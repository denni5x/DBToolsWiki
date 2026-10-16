# Place

Place a blueprint with a live, surface-aware preview.

## What it's for

Putting one [blueprint](../glossary.md#blueprint) where you want it, and seeing it there
before you commit. The simplest of the placement tools, and the one to reach for when you are
not placing many.

Its whole value is `Auto Rotate`: a prop placed on a slope or against a wall orients itself to
that surface, so you are not rotating by hand before every paste.

## The loop

1. **Pick a blueprint** from Axiom's blueprint browser.
2. **Hover.** With `Live Preview` on, the blueprint is drawn where it would land, following
   your cursor.
3. **Nudge** with the direction keys if the surface guess is not quite what you wanted.
4. **Place.**

## The three knobs that matter

### `Auto Rotate`

Orients the blueprint to the surface under the cursor instead of keeping its saved rotation.
Right for anything that should sit *on* something — rocks, debris, vegetation, wall fittings.

Turn it off for anything with a correct upright orientation that you do not want reinterpreted:
a building, a signpost, a statue.

### `Live Preview`

Draws the result before you commit. On by default and worth leaving on; the cost is per-frame
rendering of the blueprint, which only matters for very large ones.

### `Include Non-Solid Blocks`

Whether the preview's surface detection counts non-solid blocks — grass, flowers, water. Off,
the placement reads through them to the ground beneath, which is usually what you want when
placing onto a meadow. On, a tall grass block is a surface and the asset sits on top of it.

## Gotchas

- **`Auto Rotate` reads one surface, not the shape.** It orients to the block face under the
  cursor, so on a ridge or an edge the result depends on exactly which side you hovered. Move
  the cursor a block and try again rather than fighting it.
- **`Merge Blocks` changes what overwrites what.** With it on, the blueprint's blocks combine
  with what is already there instead of replacing the volume — the difference between dressing
  an existing build and stamping over it.
- **The direction keys move the placement, not the camera**, while a preview is live.
- **One blueprint at a time.** For many at once, use [Grid Place](grid-place.md),
  [BP→Path](bp-to-path.md) or [BP→Shape](bp-to-shape.md).

## Keys

| Action | What it does |
|---|---|
| `North` / `South` / `East` / `West` / `Up` / `Down` | Nudge the placement one block |
| `Rotate Placement` | Rotate the blueprint |
| `Flip Placement` | Mirror the blueprint |
| `Middle mouse` | Pick up / reposition |
| `Scroll` | Adjust the placement |
