# BP→Path

Scatter blueprints along a curve, oriented to it.

## What it's for

Anything that follows a line: fence posts along a boundary, lamps down a street, trees beside a
river, sleepers along a railway, rocks edging a path.

The alternative is placing each one by hand and rotating it to match the curve, which is both
slow and visibly imperfect. This draws the curve once and distributes along it.

## The loop

1. **Add your blueprints** - `Add Blueprint` from the browser, or `Add Clipboard` for what you
   just copied. Several can be added; the tool alternates between them along the path.
2. **Draw the curve** by placing points in the world. Points are draggable after the fact.
3. **Set `Distance`** - the spacing between placements.
4. **Decide on `Orient To Path`** and `Roll`.
5. **`Paste Blueprints`.**

## The three knobs that matter

### `Distance`

Spacing along the curve, and the control you will spend the most time on. Too tight and
assets interpenetrate; too loose and the line reads as dots rather than a boundary.

It is measured along the curve, so a tight bend packs placements closer in straight-line terms
than the number suggests.

### `Orient To Path`

Rotates each placement to follow the curve's direction. Essential for anything with a front —
a bench, a signpost, a carriage. Turn it off for radially symmetric assets like rocks and
bushes, where orienting them adds nothing and makes the repetition more obvious.

### `Roll`

Rotates each placement around the path's own axis. Its use is terrain-following: on a path
that climbs, `Roll` lets assets bank with the slope rather than staying stubbornly level.

`Unlock Rotation` frees the rotation axes from each other when you need an orientation the
combined controls will not otherwise reach.

## Gotchas

- **Several blueprints alternate, they do not randomise.** Add three and you get 1, 2, 3, 1,
  2, 3 along the path. That regularity is visible on a long run - for genuinely varied
  scattering use the [Biome Brush](biome-brush.md)'s object layers, which roll weighted picks.
- **`Distance` is arc length.** On a curve with a tight radius, placements that are evenly
  spaced along the path look bunched from above.
- **Spacing does not know about asset size.** Unlike the Biome Brush's `Min Spacing`, this is
  an absolute distance, so swapping a small blueprint for a large one will overlap at the same
  setting.
- **Escape does not cancel a drawn curve the way it cancels other placements** - this tool
  reports a *deselect point* action instead, so Escape steps out of the active point rather
  than discarding your path.

## Keys

| Action | What it does |
|---|---|
| `Left mouse` | Place or drag a curve point |
| `Right mouse` | Begin / adjust the path |
| `Extrude` | Extend the path |
| `Enter` | Paste the blueprints |
