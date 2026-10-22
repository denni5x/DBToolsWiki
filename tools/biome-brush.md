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
   scattered [blueprints](../glossary.md#blueprint), `Add Plant Layer` for Minecraft's own
   plants. Or load a [preset](../glossary.md#preset) from `Presets` at the top of the tool.
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

## Plant layers

A plant layer stands one plant per column, drawn from a weighted set of Minecraft's 95 plants.

It exists because plants are the one thing an object layer handles badly. A two-block plant -
tall grass, a sunflower, a large fern - is two block states that have to agree with each other,
and a scatter that writes one of them writes a broken half. A plant layer places the whole
plant, so the set can be picked from by name without thinking about it.

Weighting works as it does in an object layer: relative, not percentages, and weight 0 keeps an
entry in the list and out of the roll.

## A blueprint's own offset

Every blueprint in an object layer carries an `X`, `Y`, `Z` offset of its own, in its row
alongside the weight. The layer's `Vertical Offset` moves the whole set at once; this moves one
entry. The two add together.

`Y` is what it is for. A tree whose blueprint carries three blocks of root has to sit three
blocks lower than one that starts at its trunk, so a mixed set of trees could only ever be right
for one of them while the layer's slider was the only control.

The offset is measured in the blueprint's own frame, so it turns with the placement. A sideways
nudge that puts an off-centre trunk over its column keeps pointing the same way relative to the
object under `Random Yaw`, rather than being correct at one rotation in four.

It moves where an object lands and deliberately not which columns are candidates, so nudging an
entry changes neither the weighting nor the spacing.

Each entry is its own collapsible row, under a header that folds the whole set away. The row
shows the name, the weight and the share of the draw; open it for the weight and the offset. A
layer with seventy-six trees in it used to push the layer's own spacing and strength a screen and
a half below the list, which is the wrong way round - those are what a set of trees is actually
tuned with.

## WorldPainter interchange

`Export .layer` writes the biome as a WorldPainter custom layer: terrain layers as ground cover,
object layers as sets of custom objects, plant layers as WorldPainter's own Plants layer, all
combined into one paintable layer.

`Import .layer` reads one back, including a layer built in WorldPainter rather than here. Its
custom objects are written into your blueprint library as blueprints and arrive as an object
layer. **Imports are added to the biome rather than replacing it**, so an import never costs you
work you had already done.

Both directions report what they could not carry across rather than dropping it quietly. The
things worth knowing before you rely on a round trip:

- **Density is not spacing.** WorldPainter rolls one density percentage per column and lets the
  results overlap; the Biome Brush has a `Strength Factor` and a `Min Spacing` measured in each
  object's footprint. One number has to become two. Import puts the density on the
  `Strength Factor` and leaves `Min Spacing` at the 100% a new layer starts at, so an imported
  set is **thinner than it was in WorldPainter** - turning `Min Spacing` down is the fix, and the
  import says so.
- **A blueprint's height offset goes both ways, its sideways offset does not.** WorldPainter
  keeps a vertical offset per object, so export writes the layer's `Vertical Offset` plus each
  blueprint's own as one number and import splits them back out - whatever every object agrees on
  goes back on the layer's slider, and each blueprint keeps only its difference. WorldPainter has
  only one horizontal offset per object and uses it as that object's anchor rather than as an
  adjustment to one, so a sideways nudge has nowhere to go. Export says which blueprints were
  affected.
- **A mixed material's mode carries across.** Noise arrives as independent per-block picks, Blobs
  as patches of WorldPainter's own size. Layered is the one mode a single terrain layer cannot
  express, and the import says so.
- **A blob mix will not look identical on both sides.** The two generators are sampled at
  different scales.

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
- **An import adds, it does not replace.** Run the same `.layer` twice and you have the stack
  twice.
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
- [River](river-tool.md) - moving the surface rather than dressing it, and the same scatter
