# Concepts

The ideas every DBTools tool shares. Read this once and the tool pages get much shorter —
they assume everything here.

Individual terms are defined in the [glossary](glossary.md).

## DBTools sits on Axiom

DBTools is an Axiom add-on, not a standalone editor. That is not a disclaimer, it is how
you should read every tool: DBTools adds tools to **Axiom's** toolbar, paints through
**Axiom's** mask window, writes through **Axiom's** undo system, and reads blueprints from
**Axiom's** blueprint browser.

Practical consequences, each of which surprises someone eventually:

- **Undo is Axiom's undo.** A committed DBTools edit is taken back by the same Undo that
  takes back an Axiom edit, and the two interleave in one history.
- **Masks are Axiom masks**, edited in Axiom's Mask window. DBTools does not have a mask
  editor of its own - it borrows that window. See [Mask ownership](#mask-ownership).
- **Blueprints are Axiom blueprints.** Any tool that places one reads the same folder the
  blueprint browser shows.
- **Axiom's version matters.** DBTools tracks specific Axiom releases; a mismatch is the
  first thing to check when a tool is missing from the toolbar.

## Three surfaces

DBTools is not only a set of toolbar tools, and looking for everything on the toolbar is
how people miss half of it. There are three places it appears.

### 1. Axiom's toolbar - eleven tools

Create, Place, Rotate, BP→Path, BP→Shape, Grid Place, Tree, Prototype, OBJ to MC, Slab
Brush, Biome Brush. These are *tools*: you select one, it takes your clicks, and it writes
through Axiom's undo. Everything else on this page is mostly about them.

### 2. The main menu bar - a **DBTools** menu

Four entries, none of which is a tool and none of which needs one selected:

| Entry | What it opens |
|---|---|
| Bulk Edit Blueprints | The Bulk Blueprint Editor - rename, retag and re-author many blueprints at once |
| File Conversion | Blueprint ⇄ schematic conversion, and Hytale prefab export |
| Node Editor | A node graph for terrain and biomes |
| Settings | DBTools' own options |

These are **windows, not tools**, and the difference matters: they operate on files and
folders rather than on the world, so they have no preview, no Enter-to-commit, and no undo
step. A bulk rename is a change to your blueprint folder, and the way to undo it is a
backup.

The Node Editor is the one that behaves a little like a tool: opening it deselects Axiom's
active tool so its keybinds and clicks do not overlap the editor's.

### 3. Inside Axiom's own UI - extensions

DBTools adds controls to screens that are Axiom's, not its own, so they appear with no
DBTools branding around them:

- **Export Palette** in Axiom's creative colour picker and in its gradient helper - a side
  tab under Axiom's own that adds the blocks listed for the picked colour, or the generated
  gradient, to the Palettes panel as a new palette.
- **A second fly speed slider** in Axiom's hotbar screen, alongside the first. Toggle it in
  DBTools → Settings (`Show 2nd Fly Speed Slider`).
- **Toggle Fly Speed**, the mod's one Minecraft keybinding (unbound by default), which swaps
  the live fly speed with the second slider's stored value - so you can flip between a
  survey speed and a detail speed without dragging anything.

If you cannot find a DBTools feature, it is on one of these three surfaces, and the menu bar
is the one people forget.

## Preview, then commit

Most DBTools tools do not write blocks while you are adjusting them. They show you what
*would* happen, you keep adjusting, and then you commit - almost always with **Enter**.

This matters because it inverts the usual expectation. Dragging a slider in the Biome
Brush is free: nothing has been written, undo has nothing to take back, and you can keep
going. The moment of truth is Enter.

Two consequences:

- **Undo means different things before and after the commit.** While a preview is standing,
  there is nothing in the world to undo - so in the Biome Brush, Undo clears the painted
  mask instead, because that is the only thing on screen you could possibly mean. Once you
  have pressed Enter, Undo reaches Axiom's history and takes the blocks back.
- **Escape cancels rather than closes.** Tools that can be mid-placement report a cancel
  label to Axiom while there is something to cancel, and ignore Escape otherwise so the key
  still reaches the game menu. OBJ to MC is explicit about this: it only takes Escape and
  Delete while a placement is actually live, not for as long as a model stays loaded.

The exception is **Instant Apply** in the Biome Brush, which commits each stroke on release
and so skips the preview stage entirely. Under Instant Apply there is no mask to clear, and
Undo means what it normally means.

## Masks

A **mask** answers "where is this allowed to apply?" - on grass only, above y=70 only, where
the ground is steeper than 45°, and so on. DBTools masks are Axiom masks, built in Axiom's
Mask window.

Two things make DBTools' use of them unusual.

### A mask is tested at the surface, once per column

A DBTools mask is evaluated **once per painted column, at that column's surface** - not at
every block it is about to write. This is a deliberate choice with a visible payoff: a layer
masked to "steeper than 45°" keeps its full depth, where testing every block would leave
only the topmost row, because a block buried in the ground has no slope to compare.

For object layers it goes further: the mask gates the **whole placement**. A tree either
grows at that spot or it does not, rather than arriving with the blocks the mask rejected
cut out of it.

### Mask ownership

There is one Axiom Mask window and potentially many masks that want it - a biome's own mask,
and one per layer. DBTools resolves this by **only taking the window when you ask**: press
`Edit Biome Mask` (or a layer's `Layer Mask`) and that mask takes over the window until you
are done.

So selecting a tool never disturbs the Mask window, and a biome and each of its layers can
each hold a mask without stealing it from one another.

A mask is saved **with the thing it belongs to**, not globally. "Only on grass, only above
y=70" is part of the biome, travelling with it and its presets, rather than a global setting
you have to remember to set. A tuned layer likewise carries its mask into every biome you
reuse it in.

One exception worth knowing: the **Biome node** in the Node Editor *reports* its masks
rather than applying them, because its surfaces come from a region rather than the world,
and there is nothing to test them against.

## Layers

A biome is a **stack of layers** applied to a painted area, top to bottom. There are two
kinds, and the difference is what they write.

| | Terrain layer | Object layer |
|---|---|---|
| Writes | Blocks, in a band measured from the surface | Whole blueprints, scattered on the surface |
| Shaped by | `Offset From Surface` and `Depth` | `Min Spacing`, `Vertical Offset`, `Random Yaw`, `Random Flip` |
| Block choice | A weighted blend sampled spatially, driven by Axiom's noise painter | Picked from a weighted set of blueprints |
| Masked | Per block band, tested at the surface | Per placement - all or nothing |

Both kinds share the stack controls: `Enabled`, `Solo`, `Remove`, `Move Up`, a collapsible
header, and a `Strength Factor`.

A terrain layer's band can start **above** the surface as well as below, so `Offset From
Surface` runs -64 to 64.

Within an object layer, the whole set shares one `Min Spacing`, which is why variants of the
same object keep their distance from each other rather than interpenetrating. Each entry has
a weight deciding how often it is picked; **weight 0 leaves an entry out of the roll** without
removing it from the list.

## Coverage, not opacity

This is the single most misread control in the mod, so it gets its own section.

Mask strength is **coverage**, not opacity. At 50% strength, half the columns under the brush
change - fully - and the other half are untouched. It is not a 50% blend of the layer with
what was there.

And the choice of *which* columns is a **function of position**, not a random draw. So the
dither pattern does not crawl or shimmer while you drag a slider: the same column keeps the
same answer. That stability is the whole reason it is computed this way.

`Strength Factor` on an individual layer scales the mask under **that layer alone**. This is
what lets one painted mask drive full ground cover and sparse trees at the same time: the
terrain layer reads the mask at full strength, the tree layer reads it scaled down.

## Presets

Presets are stored as NBT. Built-in defaults ship inside the mod as resources (the Tree Tool's
live under `tool_presets/tree_tool/`), and your own saved presets sit alongside them.

What a preset does and does not carry is a deliberate line:

- **Carried:** the layer stack, each layer's parameters, each layer's mask, the biome's own
  mask, and the Minecraft biome setting.
- **Not carried:** the painted column mask. That is selection state - where you happen to be
  working right now - not part of the biome's definition.

Object layer blueprints are saved as **path + name + weight** rather than embedded. An entry
that no longer resolves is kept as a "not found" placeholder with a `Reselect` button, and is
never rolled into the scatter. A renamed blueprint folder therefore degrades to something you
can fix, rather than silently dropping objects or refusing to load.

## Keys

DBTools adds exactly **one** Minecraft keybinding: **Toggle Fly Speed**, which is *unbound by
default* - set it in Options → Controls → DBTools if you want it.

Everything else is not a keybinding at all. Tools receive Axiom's **user actions**, so Enter,
Delete, Escape, Undo, Redo and the mouse buttons mean what they mean in Axiom, and a tool only
claims one when it has something to do with it. A tool that does not claim a key lets it reach
the game.

Tool selection is likewise Axiom's. The only tool with a number key of its own is
**Prototype**, on `9`.

Each tool page lists the actions that tool actually claims. The common ones:

| Action | Usual meaning in DBTools |
|---|---|
| `Enter` | Commit the preview |
| `Right mouse` | Begin a stroke or a placement |
| `Escape` | Cancel the live placement, if there is one |
| `Delete` | Clear the preview or mask |
| `Undo` | Clear a standing mask if one is up, otherwise Axiom's undo |

## Where files live

| What | Where |
|---|---|
| Config | `config/dbtools/dbtools.json` |
| Built-in tool presets | Inside the mod, e.g. `tool_presets/tree_tool/*.nbt` |
| Blueprints | Axiom's blueprint folder, as shown in its browser |
| Blueprint output folder | Set per-tool in the Create Tool (`Set Blueprint Folder`) |
| Conversion input/output | Set per-tab in File Conversion (`Set Folder`, `Set Source Folder`, `Set Output Folder`) |
| Hytale block mapping | Default ships with the mod; `Export Default Mapping` writes it out, `Select Custom Mapping File` overrides it |
