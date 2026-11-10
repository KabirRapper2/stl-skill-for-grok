---
name: stl
description: Generate 3D-printable models (.stl or OpenSCAD source) from natural language prompts. Triggered by /stl, make me a 3d, create a 3d printable, generate stl, or similar requests for 3D printables.
---

# STL — Generate 3D Printable Models

When the user asks for a 3D printable object (via `/stl`, "make me a 3d ...", "create an stl of ...", etc.), follow this workflow.

## Goal

Produce a usable 3D model the user can download and print. Prefer delivering both:
1. An OpenSCAD source file (`.scad`) — editable and reliable
2. A binary STL (`.stl`) when possible

## Step-by-step process

1. **Clarify the request** (only if needed)
   - Dimensions, wall thickness, intended use (functional part, decorative, enclosure, etc.)
   - Material / print orientation constraints if mentioned
   - Keep questions minimal — default to sensible values when the user is vague.

2. **Choose generation method**
   - **Preferred**: Write clean, well-commented **OpenSCAD** code.
     - Use parametric variables at the top.
     - Include `$fn` for resolution.
     - Add a short usage comment.
   - **Simple geometry only**: If the object is a basic solid (box, cylinder, sphere, extruded polygon, etc.), also generate a mesh with Python + `numpy-stl` and export a real `.stl`.
   - Complex organic / freeform shapes: explain that true generative 3D AI is not available and deliver the best parametric OpenSCAD approximation + suggestions.

3. **Write the files**
   - Save the OpenSCAD source as `/home/workdir/artifacts/<name>.scad`
   - If generating STL, save as `/home/workdir/artifacts/<name>.stl`
   - Use clear, descriptive filenames (e.g. `phone-stand.scad`, `cable-clip.stl`).

4. **Deliver to the user**
   - Briefly describe what was created and any assumptions made (size, orientation, etc.).
   - Render the files using the available file render component so the user can download them.
   - Give short print tips when useful (recommended orientation, support needs, layer height).

## OpenSCAD guidelines

- Always start with a clear header comment.
- Put all dimensions in variables at the top.
- Use `difference()`, `union()`, `hull()`, `minkowski()` appropriately.
- Add `$fn = 64;` (or higher) for curved surfaces.
- Keep the code readable and reasonably short.

## Python STL generation (simple cases only)

Use this pattern when the geometry is elementary:

```python
import numpy as np
from stl import mesh

# Define vertices and faces, then
your_mesh = mesh.Mesh(np.zeros(faces.shape[0], dtype=mesh.Mesh.dtype))
your_mesh.vectors = ...
your_mesh.save('/home/workdir/artifacts/name.stl')
```

Only attempt this for boxes, cylinders, simple extrusions, or basic combinations. Do not invent complex freeform meshes.

## Limitations (be honest)

- No access to full CAD software or generative 3D AI models.
- Organic / highly detailed artistic models cannot be produced as high-quality STLs.
- For complex parts, deliver the best OpenSCAD approximation and note that the user can refine it in FreeCAD / OpenSCAD / Tinkercad.

## Example triggers

- `/stl phone stand for iPhone 15`
- "Hey can u make me a 3d printable cable clip"
- "generate an stl of a simple desk organizer"
- "create a 3d model of a custom switch plate"

Always stay practical, safe for 3D printing (reasonable wall thicknesses, no impossible overhangs without warning), and transparent about what the model actually contains.
