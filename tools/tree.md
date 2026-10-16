# Tree

A parametric tree, every parameter live.

## What it's for

Growing a tree to spec rather than placing one from a library. Trunk thickness, bend, branch
density, twig count, leaf shape, roots — all parameters, all adjustable with the tree redrawing
as you drag.

It is the tool with the most controls in DBTools by a wide margin, which makes the
[preset](../glossary.md#preset) list the thing to reach for first. Start from a preset, change
three things.

## The loop

1. **Load a preset** from the top of the tool. There are built-in defaults shipped with the
   mod.
2. **Place** the tree where you want it.
3. **Adjust** — every parameter redraws the tree live.
4. **`Enter`** to commit.

The panel is organised by part: `Trunk/ Root`, `Branch`, `Twig`, `Leaf`, plus `Blocks`,
`Cross-Section`, `Rotation`, `Noise Variation` and `Deferred Generation Options`.

## The three knobs that matter

### Scale

The single most useful control, and the one most worth dragging first — it regrows the whole
tree proportionally, so you can find the size before tuning anything else.

With smoothing on, holding the scale slider **defers the recalculation to release** rather than
recomputing every frame, so the drag stays responsive. Every other parameter follows the drag
live.

### `Enable Noise Variation` (and `Randomize Noise Seed`)

Without variation, a tree is deterministic — and two of the same preset side by side are
visibly identical. Noise variation breaks that up, and `Randomize Noise Seed` gives you a
different tree from the same parameters.

This is what makes the tool usable for a forest rather than a specimen: same preset, new seed
per tree.

### `Deferred Generation Options`

`Deferred Trunk`, `Deferred Branches`, `Deferred Twigs`, `Deferred Roots` — each defers that
part's regeneration until you release the slider.

On a large tree with smoothing, regenerating twigs every frame is what makes the panel feel
sluggish. Defer the parts you are not currently tuning.

## Gotchas

- **Start from a preset.** The parameter count is high enough that building a plausible tree
  from defaults is genuinely harder than adjusting an existing one.
- **Scale with smoothing is deferred, other parameters are not.** If the tree seems not to
  respond while you drag scale, that is the deferral, not a freeze — it updates on release.
- **Same preset, same tree.** Without `Enable Noise Variation` and a fresh seed, every
  placement is identical. A row of identical trees is the most common complaint, and the seed
  is the fix.
- **Leaf count drives cost more than anything else.** `Number Of Leaves` multiplied across
  `Twigs Per Branch` and `Branches Per Segment` is where the block count comes from — a tree
  that stalls is usually a twig/leaf multiplication problem, not a trunk one.
- **Roots need somewhere to go.** Root parameters on a tree placed on flat stone have nothing
  to spread into and will look wrong; roots read best on uneven or soft ground.

## Keys

| Action | What it does |
|---|---|
| `Left mouse` / `Right mouse` | Position the tree |
| `Rotate Placement` | Rotate the tree |
| `Enter` | Commit the tree |
