# OBJ to MC

Turn a 3D model into colour-matched blocks.

## What it's for

Importing geometry built elsewhere. Despite the name it accepts **OBJ, glTF/GLB, STL and PLY**,
all through one `Browse Model` dialog, and matches the model's colours to Minecraft blocks.

Colour comes from whatever the format carries: textures and MTL materials, vertex colours
(PLY, glTF `COLOR_0`), and glTF base colour factors.

## The loop

1. **`Browse Model`** (and `Browse MTL` if an OBJ needs one).
2. **Size it** — drag `Size`, or the per-axis `Scale X/Y/Z` with `Unlock XYZ Scale` on.
3. **Orient it** with `Rot X/Y/Z`, or the gizmo.
4. **Tune the match** — `Fill Mode`, `Brightness`, `Hue`, the block filters.
5. **Turn on `Enable Block Smoothing`** and tune it.
6. **Position and `Place (Enter)`.**

`Copy to Clipboard` takes the result into Axiom's clipboard instead of placing it; `Reset`
starts over.

## The three knobs that matter

### `Enable Block Smoothing`

The single biggest quality lever. Off, every voxel is a full block and the model is blocky. On,
the model is voxelised at double resolution and rebuilt with colour-matched **stairs, slabs,
walls and trapdoors**, so curves and diagonals resolve.

`Prefer Trapdoors` places a trapdoor wherever one fits, for thinner detail.
`Custom Smoothing Blocks` restricts which shapes may be used — an empty list disables that
shape entirely.

### `Brightness` (and `Hue`)

`Brightness` is a gain applied to every sampled colour **before** matching. It exists because a
model's texture often does not carry the lighting the model was rendered with, so the raw
colours match to blocks that look flat or muddy. Applied on the plain, smoothed and interior
paths alike.

`Hue` rotates every sampled colour's hue up to 180° either way, leaving saturation and
lightness alone — so greys stay grey. Use it to shift a model's palette into blocks Minecraft
actually has.

### `Limit Smoothing by Colour` / `Max Colour Error`

Smoothing picks the closest *shape*, which sometimes means accepting a worse *colour*. This sets
a maximum colour error past which a cell takes a full block instead — trading smooth edges for
accurate colour.

Turn it on when a smoothed model has gone the wrong colour in places; raise `Max Colour Error`
to favour smoothness, lower it to favour colour.

## Gotchas

- **`Back Thin Layers` fixes see-through surfaces.** A thin single-layer surface rebuilt from
  slabs and trapdoors can be transparent where half-layers meet across a diagonal. This puts a
  full block under every slab, stair and flat trapdoor that has nothing beneath it. If an
  imported sheet or wing looks perforated, this is the control.
- **Scale past 10× is not available, and past 400 voxels it warns.** `Size` and the per-axis
  sliders reach 10×, and once the longest axis passes 400 voxels the tool reports it — that is
  where revoxelising starts to cost real time.
- **Escape and Delete are only claimed while a placement is live**, so they reach the game menu
  when a model is merely loaded. Cancelling re-arms the preview rather than dropping the model.
- **The block filters are exclusions, not a palette.** `No Ores`, `No Glazed Terracotta`,
  `No Tile Entities`, `Solid Only`, `Opaque`, `Full Cube`, `Same Texture` narrow what the
  matcher may use. For positive control, use `Custom Palette`.
- **`Fill from Auto Result` is the shortcut worth knowing.** It fills the block palette and the
  four smoothing lists with the blocks the automatic match actually placed, most-used first —
  far faster than curating a palette by hand.
- **Interior voxels are written as full blocks and never smoothed**, which is why a solid model
  converts much faster than its voxel count suggests.
- **`Export Palette`** adds the model's matched full blocks to Axiom's Palettes panel as a new
  palette named after the model. It sits inside the `Custom Palette` section while that is on.

## Keys

| Action | What it does |
|---|---|
| `Left mouse` | Position the placement / drag the gizmo |
| `Enter` or `Paste` | Place the blocks |
| `Escape` or `Delete` | Cancel the live placement (re-arms the preview) |
