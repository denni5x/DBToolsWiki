# Glossary

DBTools has invented vocabulary, and most recurring support questions are really a term
being read the obvious way rather than the intended way. Each term is defined once here;
the tool pages lean on these definitions instead of re-explaining.

Background for the ideas behind several of these: [Concepts](concepts.md).

---

### Blueprint

An Axiom blueprint — a saved chunk of build, read from Axiom's blueprint browser. DBTools
does not have a blueprint format of its own. The Create Tool *writes* blueprints; Place,
Rotate, BP→Path, BP→Shape, Grid Place, Prototype and the Biome Brush's object layers *read*
them.

In an object layer a blueprint is stored as **path + name + weight**, not embedded — see
[preset](#preset).

### Column mask

The area you paint with the Biome Brush. It is called a *column* mask because it is keyed by
(x, z): each painted position records the surface Y the stroke actually reached, and the
biome is applied there.

It is **selection state, not definition** — it is deliberately left out of presets, and
clearing it throws away where you were working, not what you had built.

Do not confuse it with a [mask](#mask), which is a rule about where applying is *allowed*. The
column mask is the area; the mask is the filter on it.

### Coverage (vs opacity)

Mask strength is coverage. At 50%, **half the columns under the brush change completely** and
the rest are untouched. It is not a half-strength blend.

Which columns change is a function of position, so the pattern holds still while you drag a
slider rather than crawling. See [Coverage, not opacity](concepts.md#coverage-not-opacity).

### Defer Preview

A Biome Brush option. While you paint, show the mask as a **selection overlay** instead of the
resulting blocks, and work the blocks out once when you release.

The result is identical; the point is cost. Adding a single block to a region chunk re-meshes
the whole of it, and a radius-30 brush straddles twenty chunks at once, so previewing blocks
live is what makes a wide brush expensive. The overlay includes what you had already brushed,
so it answers "where have I painted" rather than only "where am I painting now". Erase strokes
preview as they always did.

### Instant Apply

A Biome Brush option: commit each stroke as soon as you release the button, instead of painting
a [column mask](#column-mask) to preview and pressing Enter.

Under Instant Apply there is no standing mask, so Undo means Axiom's undo and takes the
committed blocks back.

### Layer

One entry in a biome's stack. Either a [terrain layer](#terrain-layer) or an
[object layer](#object-layer). Layers apply top to bottom and each can carry its own
[mask](#mask) and [strength factor](#strength-factor).

### Mask

A rule about **where applying is allowed** — on grass, above y=70, steeper than 45°. Built in
Axiom's Mask window.

Tested **once per column at that column's surface**, not per block, so a layer masked to a
slope keeps its full depth instead of collapsing to its top row. For object layers it gates the
whole placement.

A biome has one, and so can each of its layers, narrowing the biome's rather than replacing it.
See [Masks](concepts.md#masks).

### Object layer

A [layer](#layer) that scatters whole [blueprints](#blueprint) across the masked surface, rather
than writing blocks. The set shares one `Min Spacing`, expressed as a percentage of the object's
own footprint, so variants of the same object keep their distance from each other instead of
interpenetrating. Also carries `Vertical Offset`, `Random Yaw` and `Random Flip`.

Masked all-or-nothing: a tree either grows there or does not.

### Preset

A saved tool configuration, stored as NBT.

For the Biome Brush a preset carries the layer stack, every layer's parameters and mask, the
biome's own mask, and the Minecraft biome setting — but **not** the painted
[column mask](#column-mask). See [Presets](concepts.md#presets).

### Solo

A per-layer toggle that shows **only** that layer, temporarily ignoring the rest of the stack.
Borrowed from audio mixing, and means the same thing: it is for hearing one thing on its own
while you tune it, not a property of the layer.

Lives on the layer header, so it stays reachable when the layer is collapsed.

### Strength factor

A per-[layer](#layer) scale on the [column mask](#column-mask), applied to that layer alone.

This is how one painted mask drives full ground cover and sparse trees at once: the terrain
layer reads the mask as painted, the tree layer reads it scaled down. It scales
[coverage](#coverage-vs-opacity), so lowering it thins out *which* columns that layer touches.

### Terrain layer

A [layer](#layer) that replaces a **band of blocks measured from the surface** — `Offset From
Surface` plus `Depth`. The offset runs -64 to 64, so a band can start above the surface as well
as below.

The block mix is driven by Axiom's noise painter, which makes the layer a weighted blend of
blocks sampled spatially rather than a single block choice.

### User action

Axiom's input abstraction. DBTools tools do not register keybindings for Enter, Delete, Escape
and the mouse buttons — they receive Axiom's user actions and claim the ones they have a use
for, letting the rest reach the game.

DBTools registers exactly one real Minecraft keybinding: Toggle Fly Speed, unbound by default.
See [Keys](concepts.md#keys).
