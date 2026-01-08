# Multi-View Alignment Guide for ComfyUI Text-To-3D-Mesh

## Overview

This guide explains how to achieve better multi-view alignment when generating 3D meshes from text prompts using Stable Diffusion 3.5 and Hunyuan3D-2mv.

## Problem Statement

The original workflow generates four independent views (front, back, left, right) using randomized seeds, which can lead to:
- **Inconsistent features** across different angles
- **Different lighting or shadows** in each view
- **Misaligned proportions** between views
- **Variations in details** (textures, colors, shapes)

These inconsistencies cause the Hunyuan3D-2mv model to produce lower-quality 3D meshes with artifacts or distorted geometry.

## Solutions Implemented

### 1. Fixed Seed Across All Views

**What it does:** Uses the same seed value for all four KSampler nodes instead of "randomize"

**Why it helps:** Ensures that the base noise pattern is consistent across all views, leading to more coherent generation of the same object from different angles.

**How to use:**
- All KSampler nodes (Front, Left, Right, Back) now use seed `123456789` with "fixed" control
- To try different variations, change this seed value to the same number in all four samplers
- Keep the seed synchronized across all views for best results

### 2. Orthographic View Prompts

**What it does:** Adds "Orthographic" prefix to view descriptions

**Why it helps:** 
- Orthographic projection eliminates perspective distortion
- Makes views more suitable for 3D reconstruction
- Creates consistent scale across all angles
- Reduces depth ambiguity

**Prompts used:**
- "Orthographic front view of [object]"
- "Orthographic left side view of [object]"
- "Orthographic right side view of [object]"
- "Orthographic back view of [object]"

### 3. Increased Sampling Steps

**What it does:** Increased from 20 to 30 steps

**Why it helps:** More denoising steps allow the model to:
- Refine details more carefully
- Reduce artifacts
- Better follow prompt instructions
- Create more coherent images

### 4. Optimized CFG Scale

**What it does:** Adjusted from 4.0 to 5.5

**Why it helps:** 
- Higher CFG (Classifier Free Guidance) makes the model follow prompts more closely
- 5.5 is a sweet spot for SD3.5 - strong enough for accuracy, not too high to cause artifacts
- Ensures view-specific keywords ("front", "left", etc.) are respected

### 5. Enhanced Negative Prompts

**What it does:** Expanded negative prompt to explicitly avoid common issues

**New negative prompt includes:**
```
No floating region.
No background artifacts.
No abnormalities.
Nothing that can't be 3D printed.
No really breakable parts like whiskers or dangling hair.
No perspective distortion.
No shadows or lighting inconsistency.
No multiple objects.
No text or watermarks.
No asymmetry when it should be symmetrical.
```

**Why it helps:**
- Explicitly prevents common causes of multi-view inconsistency
- Guides the model away from lighting variations
- Ensures clean backgrounds for better 3D reconstruction
- Maintains geometric integrity

## Best Practices for Prompting

### Good Prompt Structure

```
[Object type] [key features], centered, clean background, consistent lighting
```

### Examples of Good Prompts

✅ **Good:**
- "A detailed medieval sword with ornate crossguard and pommel, centered, clean background, consistent lighting"
- "A ceramic coffee mug with smooth handle, matte finish, centered, neutral backdrop"
- "A wooden chess piece knight, carved details, centered on white background"

❌ **Avoid:**
- "A sword in a forest with dramatic lighting" (inconsistent backgrounds/lighting)
- "A coffee mug on a table with shadow" (shadows cause inconsistency)
- "Multiple chess pieces" (multi-object scenes are hard to align)

### Prompt Tips

1. **Be specific about the object** but avoid environmental details
2. **Include "centered"** to keep the object in the same position across views
3. **Specify "clean/neutral/white background"** to avoid background variations
4. **Add "consistent lighting"** to prevent shadow differences
5. **Describe symmetrical objects clearly** to leverage the model's understanding
6. **Avoid clothing on characters** unless necessary (causes alignment issues)
7. **Keep proportions simple** - complex shapes are harder to align

## Workflow Variations

### ComfyUI-Text-To-3D-Mesh-Improved-Alignment.json

This is the main improved workflow with:
- Fixed seed synchronization
- Orthographic prompts
- 30 sampling steps
- CFG scale 5.5
- Enhanced negative prompts

**Best for:** General use, most objects

### Original Workflow

The original workflow with randomized seeds is still available if you need:
- More variation in generation
- Artistic interpretation over technical accuracy
- Quick iterations

## Advanced Techniques

### 1. Manual Seed Selection

Try multiple seed values to find the best base generation:
```
123456789  (default)
987654321
555555555
100000000
```

Change all four KSampler nodes to the same seed value.

### 2. Adjusting CFG Scale

- **Lower (4.0-5.0):** More creative, less strict prompt following
- **Medium (5.5-6.5):** Balanced (recommended)
- **Higher (7.0-8.0):** Very strict prompt following, may introduce artifacts

### 3. Custom View Descriptions

For special cases, you can modify the view descriptions:

**Standard:**
- "Orthographic front view of"

**Alternative for technical objects:**
- "Technical orthographic front elevation of"
- "Blueprint front view of"
- "Schematic front projection of"

**For organic objects:**
- "Orthographic frontal view of"
- "Front-facing view of, centered, no perspective"

## Model Selection

### Stable Diffusion 3.5 Large

**Strengths:**
- Excellent prompt understanding
- Good geometric consistency
- High detail quality

**Settings:**
- Works well with CFG 5.5
- Benefits from clear, structured prompts
- Supports technical vocabulary (orthographic, elevation, etc.)

### Hunyuan3D-2mv

**Strengths:**
- State-of-the-art multi-view to 3D conversion
- Robust to minor inconsistencies
- Fast generation (30 seconds typical)

**Settings in workflow:**
- Resolution: 3072 for high quality (node 18)
- Steps: 20 (node 24)
- CFG: 7.5 (node 24)
- Voxel resolution: 64000, Grid size: 512 (node 25) for HD output

## Troubleshooting

### Issue: Views still don't align well

**Solutions:**
1. Verify all four KSamplers have identical seeds
2. Try a different seed value (some seeds work better for certain objects)
3. Simplify your prompt - remove environmental details
4. Ensure "centered" and "clean background" are in the base prompt

### Issue: Object looks different in each view

**Solutions:**
1. Check that CFG scale is the same across all samplers (5.5)
2. Verify steps are identical (30)
3. Add more specific object descriptions
4. Strengthen negative prompt

### Issue: 3D mesh has holes or artifacts

**Solutions:**
1. Improve source images by adjusting seed
2. Increase voxel resolution in node 25 (but requires more VRAM)
3. Adjust the threshold in VoxelToMesh node (currently 0.6)
4. Try simpler object shapes

### Issue: Object is cut off in some views

**Solutions:**
1. Add "full body" or "complete object" to prompt
2. Add "not cropped" to prompt
3. Add "cropped edges" to negative prompt
4. Adjust EmptySD3LatentImage size if needed (node 2)

## Performance Considerations

### VRAM Requirements

- **SD3.5 Generation:** ~8-10 GB (four parallel 1024x1024 images)
- **Hunyuan3D Processing:** ~11.5 GB (shape generation)
- **High-res mesh:** Up to 24.5 GB (with texturing)

**For lower VRAM:**
1. Generate views sequentially (disable 3 of 4 KSamplers temporarily)
2. Reduce resolution in node 2 (1024x1024 → 768x768)
3. Lower voxel resolution in node 25

### Generation Time

- **SD3.5 (4 views):** ~2-4 minutes on RTX 3090
- **Hunyuan3D:** ~30 seconds
- **Total:** ~3-5 minutes per complete 3D mesh

## Future Improvements

Potential enhancements for even better alignment:

1. **ControlNet integration:** Use depth maps to enforce geometric consistency
2. **IP-Adapter:** Maintain style consistency across views
3. **View interpolation:** Generate additional angles (45°, 135°, etc.)
4. **Reference image support:** Start from a single image and generate consistent views
5. **Automatic seed optimization:** Test multiple seeds and select best alignment

## Conclusion

The improved workflow significantly enhances multi-view alignment through:
- Synchronized seed generation
- Orthographic projection prompts
- Optimized sampling parameters
- Enhanced negative prompts

For best results:
- Use clear, centered object descriptions
- Keep backgrounds clean and neutral
- Leverage the fixed seed for consistency
- Iterate on seed values to find optimal results

## Resources

- [Hunyuan3D-2mv Model Card](https://huggingface.co/tencent/Hunyuan3D-2mv)
- [Stable Diffusion 3.5 Documentation](https://stability.ai/)
- [ComfyUI Documentation](https://docs.comfy.org/)
