# Biome Brush

Paint an area, stack layers of blocks and objects on it, and tune the whole thing live before
committing a single block.

Terms used here are defined in the [glossary](../glossary.md); the ideas behind them are in
[Concepts](../concepts.md).

## What it's for

Dressing terrain that already has its shape. You have a hillside, and you want stone where it
is steep, grass where it is not, and trees only on the gentle parts - as one continuous
surface rather than three separate passes.

The brush paints a [column mask](../glossary.md#column-mask), the biome's
[layers](../glossary.md#layer) apply to it, and nothing is written to the world until you press
Enter. So the expensive part - deciding what the biome should be - happens while nothing is
committed and everything is still adjustable.

It is not a terrain-shaping tool. It writes blocks at and around an existing surface; it does
not raise or lower that surface.

## The loop

1. **Select the tool** and set `Mode` to `Paint`.
2. **Build the stack.** `Add Terrain Layer` for bands of blocks, `Add Object Layer` for
   scattered [blueprints](../glossary.md#blueprint). Or load a
   [preset](../glossary.md#preset) from `Presets` at the top of the tool.
3. **Paint** with right mouse. The masked area fills in with the result as you go.
4. **Tune.** Drag any layer parameter and the whole painted area updates. This is free —
   nothing has been written yet.
5. **Narrow it, if needed.** `Edit Biome Mask` for the biome, or a layer's `Layer Mask` for just
   that layer. Both apply live: narrow the mask and the painted area re-narrows.
6. **Commit with Enter.** The blocks are written as an Axiom undo step, and the painted mask is
   cleared so the preview does not immediately repaint what you just committed.

Repeat from 3. The stack survives, so the second patch of hillside costs you one stroke.

To skip the preview stage entirely, turn on
[`Instant Apply`](../glossary.md#instant-apply) and each stroke commits on release.

## The three knobs that matter

Everything else is detail. These three decide whether the result looks designed or looks
generated.

### `Strength` - how much of the brush lands

Coverage, **not** opacity. At 0.50, half the columns under the brush change completely and the
rest are untouched; it is not a half-strength blend. Which columns change is a function of
position, so the pattern holds still while you drag other sliders rather than crawling.

Use it for edges. A full-strength core with a few strength-0.3 passes around the rim reads as a
natural boundary; a hard-edged full-strength patch reads as a paint bucket.

### `Strength Factor` - per layer, on the same mask

Scales the mask under **one layer alone**. This is the knob that makes a single painted stroke
produce a layered result: terrain layer at full strength for continuous ground cover, tree layer
at 0.1 so trees are scattered rather than a forest.

If you find yourself painting the same area twice with different stacks, this is the control you
wanted.

### `Offset From Surface` and `Depth` - where a terrain band sits

Together they define the band a terrain layer replaces, measured from the surface. `Offset`
runs -64 to 64, so a band can start **above** the surface as well as below - which is how you
get snow sitting on top rather than replacing the ground under it.

Depth interacts with masks in a way worth knowing: because a mask is tested once at the surface,
a masked layer keeps its full depth on a slope instead of collapsing to its topmost row.

## Gotchas

- **Enter clears the painted mask.** This is deliberate: the preview would otherwise keep
  repainting the columns just committed. Delete and the `Clear Mask` button do the same thing.
- **Undo means two different things.** With a mask standing, Undo *clears the mask* - nothing has
  been written to the world yet, so the painted mask is the only thing an undo could mean. With
  no mask up, Undo reaches Axiom's own and takes the committed blocks back. Under `Instant Apply`
  it is always the latter.
- **The brush is a sphere, not a column.** It paints the surfaces inside the brush and records
  the Y each stroke actually reached. So painting beside a cliff or under an overhang lands the
  layers on the surface you are brushing, not on the cliff top above it.
- **The painted mask is not in presets.** Saving a preset saves the stack, the masks and the
  Minecraft biome - not where you painted. That is selection state.
- **A missing blueprint does not break the layer.** An object layer entry whose blueprint no
  longer resolves becomes a "not found" placeholder with a `Reselect` button, and is skipped
  rather than rolled.
- **Weight 0 is not the same as removing an entry.** It stays in the list and out of the roll,
  which is what you want while comparing variants.
- **A wide brush is expensive for a real reason.** Adding one block to a region chunk re-meshes
  all of it, and a radius-30 brush straddles roughly twenty chunks. Turn on
  [`Defer Preview`](../glossary.md#defer-preview) and the blocks are worked out once on release
  instead of every frame.
- **Minecraft biomes snap to a 4×4×4 grid.** With `Set Minecraft Biome` on, a stroke's edge
  snaps to that grid, and it is written as a *second* undo step. There is no fill-vertically
  option: the cell at the surface is the one that gets written.
- **The Node Editor's Biome node reports masks, it does not apply them.** Its surfaces come from
  a region rather than the world, so there is nothing to test a mask against.

## Keys

Actions this tool claims, in Axiom's terms (see [Keys](../concepts.md#keys)):

| Action | What it does |
|---|---|
| `Right mouse` | Paint or erase a stroke, per `Mode` |
| `Enter` | Commit the biome to the painted area |
| `Delete` | Clear the painted mask |
| `Undo` | Clear the painted mask if one is up, otherwise Axiom's undo |
| `Escape` | Cancel the stroke in progress |

No Minecraft keybinding of its own, and no number key - select it from Axiom's toolbar.

## Related

- [Concepts → Masks](../concepts.md#masks) - why masks are tested at the surface
- [Concepts → Coverage, not opacity](../concepts.md#coverage-not-opacity)
- [Concepts → Layers](../concepts.md#layers) - terrain vs object layers side by side
