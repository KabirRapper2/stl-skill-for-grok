# stl — Grok Skill for 3D Printable Models

Generate 3D-printable models (`.stl` or OpenSCAD source) from natural language prompts.

## Trigger

- `/stl ...`
- "make me a 3d ..."
- "create a 3d printable ..."
- "generate stl of ..."

## How to use with Grok

1. Copy the `stl/` folder into your Grok skills directory (`~/.grok/skills/stl/` or the equivalent path).
2. Restart / reload skills if needed.
3. Use the triggers above.

## Example

```
/stl simple phone stand for iPhone 15
```

The skill will produce an OpenSCAD file (and a basic STL when the geometry is simple enough).

## Notes

- Works best for parametric / mechanical parts (stands, clips, organizers, brackets, enclosures).
- Complex organic shapes are limited — the skill will give the best parametric approximation and note the limitation.
- Always review the model before printing.

Created for Grok by KabirRapper2.
