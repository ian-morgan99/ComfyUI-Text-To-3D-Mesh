# ComfyUI-Text-To-3D-Mesh
ComfyUI - Text to 3D Mesh Using StableDiffusion 3.5 and Hunyuan Multiple Images to Mesh Models

This model uses the Stable Diffusion 3.5 Model that is provided as part of the ComfyUI default templates, and the Hunyuan Image to 3D Mesh Template.

## 🚀 Quick Start

New to this workflow? See **[QUICK_START.md](QUICK_START.md)** for a 5-minute setup guide.

## Workflows Available

There are three versions:

### 1. **ComfyUI-Text-To-3D-Mesh-Improved-Alignment.json** (⭐ RECOMMENDED)
**Best for:** Accurate multi-view alignment and high-quality 3D meshes

This is the improved workflow with enhanced multi-view alignment featuring:
- **Fixed seed synchronization** across all views for consistency
- **Orthographic view prompts** to eliminate perspective distortion  
- **Optimized sampling parameters** (30 steps, CFG 5.5)
- **Enhanced negative prompts** to prevent common alignment issues
- **Single prompt input** - automatically generates front, back, left, and right views

**Usage:**
- Edit the text in node 29 (PrimitiveStringMultiline) with your object description
- Keep descriptions centered and mention clean backgrounds
- Uses high-resolution 3D output (64000 voxels, 512 grid)

**See [MULTI_VIEW_ALIGNMENT_GUIDE.md](MULTI_VIEW_ALIGNMENT_GUIDE.md) for detailed usage instructions and best practices.**

### 2. ComfyUI-Text-To-3D-Mesh-High-Definition-Single-Prompt.json
**Best for:** Original workflow with randomized generation

The high resolution version uses a slightly different node for the 3D representation.
It requires only a single prompt, describing the item in as much detail as possible and will produce a GLB file.
It then creates 4 images for the front back left and right and uses those to develop the 3D model file.

### 3. ComfyUI-Text-To-3D-Mesh (Legacy)
**Best for:** Lower resolution, four separate prompts

Provide the workflow with a description of the item you want to 3D model, from the front, back, left and right, and it will generate 4 images from that text, and feed them into the Hunyuan engine.

## Multi-View Alignment

For best results achieving accurate alignment between front, back, left, and right views:

1. **Use the Improved Alignment workflow** (ComfyUI-Text-To-3D-Mesh-Improved-Alignment.json)
2. **Read the comprehensive guide:** [MULTI_VIEW_ALIGNMENT_GUIDE.md](MULTI_VIEW_ALIGNMENT_GUIDE.md)

### Key Improvements in the Enhanced Workflow

- ✅ **Synchronized Seeds:** All views use the same seed for consistency
- ✅ **Orthographic Prompts:** Eliminates perspective distortion
- ✅ **Optimized Parameters:** 30 steps, CFG 5.5 for better accuracy
- ✅ **Enhanced Negative Prompts:** Prevents lighting/shadow inconsistencies
- ✅ **Higher Quality Output:** 64K voxel resolution, 512 grid size

### Models Used

**Stable Diffusion 3.5 Large** (`sd3.5_large_fp8_scaled.safetensors`)
- Generates the four multi-view images
- Excellent prompt understanding and geometric consistency
- Works best with orthographic view descriptions

**Hunyuan3D-2mv** (`hunyuan3d-dit-v2-mv.safetensors`)  
- State-of-the-art multi-view to 3D reconstruction
- Converts four images into high-quality 3D mesh
- Typical generation time: ~30 seconds
- CLIP alignment score: 0.809 (SOTA)

For more details on how these models work together and how to optimize them, see the [alignment guide](MULTI_VIEW_ALIGNMENT_GUIDE.md).

## Example Usage

### Improved Alignment Workflow (Recommended)

**Single prompt in node 29:**
```
A detailed medieval sword with ornate crossguard and pommel, centered, clean background, consistent lighting
```

The workflow automatically generates:
- Orthographic front view of [your prompt]
- Orthographic left side view of [your prompt]
- Orthographic right side view of [your prompt]  
- Orthographic back view of [your prompt]

All with synchronized seeds and optimized settings for maximum alignment accuracy.

### Original Example (Legacy Four-Prompt Workflow)


Front view prompt:
“A highly detailed, orthographic front-on render of a medieval pewter tankard. The mug’s face is dominated by an embossed gargoyle in deep relief—its powerful wings unfurled across the body, muscular arms clutching the midsection, and a snarling visage pinned beneath the rim. Intricate chisel marks define each feather and scale on its wings and back, while the creature’s eyes and fangs are shadowed by carved recesses for dramatic contrast. The metal surface exhibits a subtle patina in the grooves, and soft rim lighting accentuates the raised gargoyle against a neutral, matte background.”

Left view prompt:
“An orthographic left-profile render of a medieval pewter tankard. The handle is sculpted into a coiled dragon’s tail, each individual scale crisply modeled and catching light at its edges. The tail loops gracefully back toward the mug’s body, ending in a pointed tip just above the base. Along the upper rim, a finely etched band of angular runic script runs the circumference, the characters inset by a fraction of a millimeter and darkened by aging patina. The pewter shows subtle hammer-textured facets across the flat plane of the side.”

Right view prompt:
“An orthographic right-profile render of a medieval pewter tankard. This side is intentionally unadorned, showcasing a gently hammered metal surface with soft irregular dents. Three small, evenly spaced rivets run vertically from just below the rim toward the base, their heads polished smooth. A mild greenish patina collects around the rivet recesses and hammer indentations, accentuating the tankard’s worn authenticity. The edge of the rim and base are slightly rounded, catching specular highlights against a dark neutral backdrop.”

Back view prompt:
“A straight-on orthographic back render of a medieval pewter tankard. The surface is completely smooth, save for two raised bands: one just below the rim and one just above the base. These concentric rings are subtly beveled and cast soft shadows across the body. The pewter displays a uniform matte finish with barely perceptible casting seams near the handle attachment point. The lighting is even and diffuse to reveal the clean geometry of the tankard’s robust cylindrical form against a plain, mid-tone background.”

Result after about 2 minutes:

![image](https://github.com/user-attachments/assets/12a8eb95-c39c-4f5c-8ef3-c2274c3d319a)

### Original High-Resolution Single Prompt Example
![image](https://github.com/user-attachments/assets/d4f24d2b-1755-4b45-819b-32ba21c06143)

## Getting Started

1. **Load the workflow** in ComfyUI
2. **Choose your workflow:**
   - Use `ComfyUI-Text-To-3D-Mesh-Improved-Alignment.json` for best results
   - Or use one of the original workflows
3. **Edit the prompt(s):**
   - For Improved Alignment: Edit node 29
   - For single-prompt workflows: Edit the prompt nodes
4. **Run the workflow** - it will generate the 3D mesh file
5. **Find your output** - `output.obj` will be created

## Tips for Best Results

✅ **DO:**
- Use the improved alignment workflow for production work
- Keep objects centered in frame
- Specify clean/neutral backgrounds
- Use consistent lighting descriptions
- Try different seed values (change all four KSamplers to same value)

❌ **AVOID:**
- Complex scenes with multiple objects
- Dramatic lighting or shadows
- Perspective distortion keywords
- Cropped or partial views
- Highly detailed backgrounds

## System Requirements

- **VRAM:** 
  - Minimum: 8GB (for SD3.5 generation)
  - Recommended: 12GB+ (for full workflow with high-res output)
  - Optimal: 24GB+ (for maximum quality)
- **Generation Time:** ~3-5 minutes total on RTX 3090
- **Output Formats:** OBJ, GLB (3D mesh files)

## Troubleshooting

**Views don't align?**
- Check that all KSampler nodes use the same seed
- Try the improved alignment workflow
- See [MULTI_VIEW_ALIGNMENT_GUIDE.md](MULTI_VIEW_ALIGNMENT_GUIDE.md)

**Low quality mesh?**
- Increase voxel resolution (node 25)
- Try different seed values
- Simplify your prompt

**Out of memory?**
- Reduce image resolution (node 2)
- Lower voxel resolution (node 25)
- Generate views sequentially instead of parallel

## Learn More

- **[Multi-View Alignment Guide](MULTI_VIEW_ALIGNMENT_GUIDE.md)** - Comprehensive guide to achieving accurate alignment
- [Hunyuan3D-2mv on HuggingFace](https://huggingface.co/tencent/Hunyuan3D-2mv) - Model documentation
- [ComfyUI Documentation](https://docs.comfy.org/) - General ComfyUI help

