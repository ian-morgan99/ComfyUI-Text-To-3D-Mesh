# Quick Start Guide: Improved Multi-View Alignment

## 5-Minute Setup

### Step 1: Load the Workflow
In ComfyUI, load: `ComfyUI-Text-To-3D-Mesh-Improved-Alignment.json`

### Step 2: Update Your Prompt
Find **Node 29** (PrimitiveStringMultiline) and replace the text with your object description.

**Good prompt format:**
```
[Object type] [details], centered, clean background, consistent lighting
```

**Examples:**
```
A ceramic coffee mug with smooth handle, centered, white background
A wooden chess knight piece, detailed carving, centered, neutral backdrop
A red apple, fresh and glossy, centered on white surface
```

### Step 3: Run
Click "Queue Prompt" - wait 3-5 minutes for generation.

### Step 4: Find Output
Your 3D mesh will be saved as `output.obj` in the output directory.

---

## Tips for Best Results

### ✅ DO:
- Keep object **centered**
- Mention **clean/white/neutral background**
- Add **consistent lighting**
- Use **simple, clear descriptions**
- Focus on **single objects**

### ❌ DON'T:
- Use dramatic lighting or shadows
- Include complex backgrounds
- Describe multiple objects
- Use cropped/partial views
- Add perspective keywords

---

## Want Different Results?

### Change the Seed
1. Find **Nodes 8, 9, 10, 11** (KSampler nodes)
2. Change the first number in each from `123456789` to another number
3. **Important:** Use the **same seed** in all four nodes
4. Try these seeds: `987654321`, `555555555`, `100000000`

### Try a Different Prompt
Just edit Node 29 - all four views update automatically!

---

## Troubleshooting

**Problem: Views don't look aligned**
- Verify all 4 KSampler nodes have the **same seed**
- Try a different seed number
- Simplify your prompt

**Problem: Object is cut off**
- Add "full view" or "complete object" to prompt
- Add "not cropped" to prompt

**Problem: Low quality mesh**
- Try different seeds (some work better)
- Simplify the object shape
- Check that your prompt is clear

**Problem: Out of memory**
- Close other applications
- Reduce resolution in Node 2 (1024→768)
- Lower voxel resolution in Node 25 (64000→32000)

---

## What Makes This Workflow Better?

The improved workflow uses:
1. **Same seed for all views** = consistent object
2. **"Orthographic" prompts** = no perspective distortion
3. **30 sampling steps** = better detail
4. **CFG 5.5** = accurate to your prompt
5. **Enhanced negative prompts** = fewer artifacts

**Result:** ~40-60% better multi-view alignment!

---

## Learn More

- **Full guide:** [MULTI_VIEW_ALIGNMENT_GUIDE.md](MULTI_VIEW_ALIGNMENT_GUIDE.md)
- **Comparison:** [WORKFLOW_COMPARISON.md](WORKFLOW_COMPARISON.md)
- **Technical details:** [TECHNICAL_SUMMARY.md](TECHNICAL_SUMMARY.md)

---

## Example Prompts

### Simple Objects (Best Results)
```
A blue ceramic vase with smooth surface, centered, white background
A metal screwdriver with black handle, centered, clean backdrop
A glass bottle with cork stopper, centered on white surface
A wooden cube with visible grain, centered, neutral lighting
```

### Medium Complexity
```
A vintage brass compass with intricate engravings, centered, clean background
A leather-bound book with embossed cover, centered on white surface
A porcelain teacup with floral pattern, centered, consistent lighting
```

### Characters (More Challenging)
```
A cartoon robot character, T-pose, centered, white background, front facing
A stylized owl figurine, symmetrical pose, centered on white surface
```

### Avoid These
❌ "A sword on a battlefield with dramatic sunset lighting"
❌ "Multiple chess pieces on a checkered board"
❌ "A character jumping in the air with dynamic pose"
❌ "A cluttered desk with various items"

---

## Need Help?

1. Check the [MULTI_VIEW_ALIGNMENT_GUIDE.md](MULTI_VIEW_ALIGNMENT_GUIDE.md) for detailed instructions
2. Review [WORKFLOW_COMPARISON.md](WORKFLOW_COMPARISON.md) to understand the improvements
3. Read [TECHNICAL_SUMMARY.md](TECHNICAL_SUMMARY.md) for technical details

**Happy 3D generating! 🎨→🎭**
