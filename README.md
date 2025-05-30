# ComfyUI-Text-To-3D-Mesh
ComfyUI - Text to 3D Mesh Using StableDiffusion 3.5 and Hunyuan Multiple Images to Mesh Models

This model uses the Stable Diffusion 3.5 Model that is provided as part of the ComfyUI default templates, and the Hunyuan Image to 3D Mesh Template.

There are two versions:

The newer, high resolution version uses a slightly different node for the 3D representation.
It requires only a single prompt, describing the item in as much detail as possible and will produce a GLB file.
It then creates 4 images for the front back left and right and uses those to develop the 3D model file.

THe older, lower resolution version works by:

Provide the workflow with a description of the item you want to 3D model, from the front, back, left and right, and it will generate 4 images from that text, and feed them into the Hunyuan engine.

Example:

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

I will now upload a higher resolution version, with a single prompt, to make it easier to use:

Example:
![image](https://github.com/user-attachments/assets/d4f24d2b-1755-4b45-819b-32ba21c06143)


