# Rotate

Rotate a pasted blueprint to any angle, snapped or free.

## What it's for

Minecraft's own rotation is 90° at a time. This tool rotates a
[blueprint](../glossary.md#blueprint) to **arbitrary angles** — 23°, 137° — and re-voxelises
the result.

It exists for the case 90° steps cannot do: a row of houses following a curved street, a fence
at the angle the terrain actually runs, a ring of pillars that are not axis-aligned.

## The loop

1. **Pick a blueprint.**
2. **Drag `Angle (°)`** and watch the preview re-voxelise.
3. **Set `Snap (°)`** if you want increments — 15° for regular arrangements, 1° for fine work.
4. **`Paste (Enter)`.**

`Reset Angle` returns to zero without reloading the blueprint.

## The three knobs that matter

### `Angle (°)`

The rotation itself. The thing to understand is that an off-axis rotation **cannot be exact**:
blocks sit on a grid, so a 37° rotation is an approximation, and diagonal edges come back
stepped. The more axis-aligned detail a blueprint has, the more obvious this is.

Shapes with organic or diagonal edges survive rotation well. Anything with long straight walls
and clean corners does not.

### `Snap (°)`

Quantises the angle. Its real use is **consistency across several pastes**: with snap at 15°,
twelve pillars around a circle all land on angles that divide evenly, which hand-dragging
cannot reliably reproduce.

### `Live Preview`

On, you see the re-voxelised result at the current angle before committing — which matters more
here than in other tools, because the stepping is the thing you are judging.

## Gotchas

- **Rotation is lossy and not reversible.** Rotating 37° and then -37° does not return the
  original blocks; each pass re-voxelises. Rotate once from the source blueprint rather than
  correcting an already-rotated paste.
- **Block states rotate as best they can.** Stairs, slabs and directional blocks only have
  90° orientations, so at 37° each is snapped to its nearest available facing. A staircase at
  an odd angle will not read cleanly.
- **The preview re-voxelises as you drag**, so a large blueprint makes the slider feel heavy.
  Set `Snap` to a coarse value while finding the angle, then refine.
- **Rotation is around the gizmo, which you position.** The pivot is the gizmo's target
  position, not the blueprint's centre — so a gizmo set off to one side makes the blueprint
  swing through an arc rather than spin in place. Check where it lands, not just how it is
  oriented.

## Keys

| Action | What it does |
|---|---|
| `Left mouse` | Position the placement / drag the gizmo |
| `Right mouse` | Begin a placement |
| `Rotate Placement` | Step the rotation |
| `Flip Placement` | Mirror the blueprint |
| `Enter` | Paste |
