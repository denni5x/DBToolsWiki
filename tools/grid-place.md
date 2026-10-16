# Grid Place

Tile a blueprint across a grid.

## What it's for

Repetition on a lattice: floor tiles, a field of crops, warehouse racking, a curtain wall of
identical window modules, a test field of forty variants laid out for comparison.

That last use is worth calling out - laying assets out in a grid to *look* at them side by side
is one of the most practical things this tool does.

## The loop

1. **Add your blueprints** with `Add Blueprint`; `Clear` empties the list.
2. **Set `Grid X` and `Grid Y`** - how many cells each way.
3. **Set `Space/Gap`** between cells.
4. **Position** the grid in the world, with `Live Preview` showing the result.
5. **Paste.**

## The three knobs that matter

### `Grid X` / `Grid Y`

The cell counts. Note that `Grid Y` is the second *horizontal* axis here, not height - this is
a floor-plan grid.

### `Override Blueprint Size`

By default, cell size comes from the blueprint's own footprint, so tiles abut exactly. Turn
this on and you set the cell size yourself.

Use it when the blueprint's bounds are not the spacing you want: an asset with an overhang
whose footprint is larger than its base, or a deliberately loose arrangement where each item
sits in a bigger cell than it fills.

### `Create Platform`

Generates a platform under the grid, using the chosen inner and outer blocks. This is what
makes the tool useful for test layouts - assets laid out on a clean surface rather than
floating or buried in terrain.

## Gotchas

- **`Grid Y` is horizontal, not vertical.** It is the second ground axis. Nothing stacks
  upward.
- **Spacing defaults to the blueprint's footprint**, so mixed-size blueprints in one list tile
  on a grid sized for whichever one drives the cell. For even layout with mixed sizes, turn on
  `Override Blueprint Size` and pick a cell big enough for the largest.
- **`Force Single Block Center` changes the origin parity.** With an even-sized footprint there
  is no true centre block; this forces one, which shifts the grid half a block. It matters when
  aligning against existing geometry.
- **Large grids are large pastes.** `Grid X` 40 × `Grid Y` 40 is 1600 blueprints in one undo
  step; fine, but it will pause.
- **`Use Relative Position`** preserves each blueprint's stored offset rather than normalising
  it into the cell, which can look like the grid is misaligned when the blueprints simply carry
  offsets.

## Keys

| Action | What it does |
|---|---|
| `Right mouse` | Begin / position the grid |
| `Enter` | Paste the grid |
| `Escape` | Cancel the grid placement |
| `Delete` | Clear the placement |
