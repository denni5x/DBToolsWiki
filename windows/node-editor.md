# Node Editor

A node graph for terrain, biomes and blueprint placement.

Opens from **main menu bar → DBTools → Node Editor**. It is the one
[menu-bar window](../concepts.md#three-surfaces) that behaves a little like a tool: opening it
**deselects Axiom's active tool**, so the editor's keybinds and clicks do not overlap a tool's.

## What it does

The tools elsewhere in DBTools each do one thing to the world directly. The Node Editor is for
composing those operations: noise drives a mask, the mask narrows a biome, the biome applies to
a region, and the result goes to an output. Change one node's parameter and everything
downstream follows.

It earns its keep in two situations: when a result needs more than one operation stacked, and
when you want to develop that stack **without touching the world** — `Test Terrain` exists
precisely so you can tune layers against a synthetic surface before pointing them at a build.

## The workflow

1. **Open it** from the menu bar. Axiom's active tool is deselected.
2. **Add nodes** and wire them together. Pins are typed, and mask pins have their own colour in
   the graph so a mask connection is distinguishable at a glance.
3. **Select a node** to see its settings in the parameters sidebar, docked to the right of the
   canvas and on by default. Double-clicking a node also opens its parameters.
4. **Tidy as you go** — `Group Selection` wraps the selected nodes in a titled box,
   `Zoom to Fit` and `Focus Selection` navigate a graph too big for the screen.
5. **Wire to `Output`** and the graph produces its result.

## The nodes

Fourteen nodes, in four categories.

| Category | Nodes |
|---|---|
| Start | `BlockPos`, `Noise` |
| Generation | `Blueprint`, `Shape`, `Test Terrain`, `Tree` |
| Transform | `Biome`, `Colorize`, `Combine`, `Displace`, `Mask`, `Mask From Noise`, `Offset`, `Position Offset` |
| End | `Output` |

### The three worth knowing first

**`Test Terrain`** is a synthetic slab with a noise-driven surface. It is scaffolding: build
your biome layers against it, confirm they behave, then swap in real geometry. Developing
layers directly against a build means committing and undoing repeatedly.

**`Biome`** applies a biome definition — the same layer stacks the
[Biome Brush](../tools/biome-brush.md) uses — to its input region. This is how a tuned biome
gets reused without painting it by hand.

**`Mask From Noise`** turns a noise pattern into a [column mask](../glossary.md#column-mask)
through a `Low`/`High` window. The window covers both jobs at once: bring `Low` and `High`
together for a hard threshold, spread them apart for a falloff.

## The options that matter

**The parameters sidebar** (toggled from the toolbar, on by default) shows the selected node's
settings beside the canvas. Before 2.4.0 this needed a double-click per node, which made
comparing two nodes' settings tedious.

**Groups** are titled boxes around a selection. On a graph of any size they are the difference
between a diagram and a tangle.

**Copy and paste** (`Ctrl+C` / `Ctrl+V`) duplicate a selection; repeated pastes cascade rather
than stacking, and preserve the cluster's internal layout. This is the fast way to build a
graph with three near-identical branches.

## Gotchas

- **The `Biome` node reports masks rather than applying them.** Both a biome's own mask and its
  layers' masks are *stated* in the node, not evaluated. Its surfaces come from a region rather
  than the world, and a mask has nothing to test against there. If you need masks to actually
  apply, paint with the [Biome Brush](../tools/biome-brush.md).
- **Opening the editor deselects your Axiom tool.** Deliberate, so clicks and keybinds do not
  collide — but it means closing the editor leaves you with no tool selected.
- **`Test Terrain` is not your world.** Layers tuned against it will behave the same way, but
  its surface is synthetic noise: it has no cliffs, no overhangs and no water edge, so a
  slope-masked layer looks better there than it will on real terrain.
- **Delete removes the selected nodes.** There is no graph-level undo, so a deleted cluster is
  rebuilt by hand — `Ctrl+C` before experimenting on something you care about.
