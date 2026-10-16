# Slab Brush

Smooth terraced terrain into slabs and stairs.

## What it's for

The staircase problem. Terrain sculpted at block resolution comes out in one-block steps, and
at a shallow gradient those steps read as terracing rather than as a slope. This brush paints
over them with half-height blocks so the gradient reads smoothly.

It is a finishing tool: shape the terrain first, then brush the steps out.

## The loop

1. **Set `Edge Mode`** and `Width`.
2. **Brush along the steps** with right mouse.
3. **Keep brushing.** There is no commit step - strokes apply as you make them.

## The three knobs that matter

### `Edge Mode`

Three ways to resolve a step, and the choice changes the terrain's height:

| Mode | What it does |
|---|---|
| `Shave high edge` | Replaces the higher edge block with a bottom slab - lowers the step |
| `Raise low edge` | Adds a bottom slab on top of the lower neighbour - raises it |
| `Both (smoothest)` | Both of the above |

`Both (smoothest)` gives the best gradient and is the default choice. The other two exist for
when the terrain's height must be preserved in one direction - shaving only, to avoid raising
ground against a wall, or raising only, to avoid cutting into a path.

### `Width`

How many blocks back from a step get smoothed. At 1 you get a single bevelled edge; raise it
and the bevel widens into a shelf, with the taper on the innermost row.

This is the control that turns a sharp terrace into a gentle bank. On terrain too steep to fit
the full width, the taper falls on the last row of the run it does have, so a short run gets a
stair rather than slabs alone.

### `Use Stairs`

With stairs on, where a step runs one way the high edge becomes a **stair** instead of a slab,
keeping the block full height on its inner side. A block that drops away on opposite sides
still gets a slab, since no stair fits.

The practical difference is that stairs preserve the terrain's mass where slabs cut it away, so
a stepped slope keeps its volume.

## Gotchas

- **No preview and no Enter.** Unlike most DBTools tools, strokes apply immediately. Undo is
  Axiom's undo.
- **It needs steps to work with.** On smooth or flat terrain there is nothing to detect and the
  brush does nothing. On a cliff face, steps are too tall for half-blocks to bridge.
- **Step detection is per block, not per edge.** So a bevel wider than one block resolves the
  same way at a corner regardless of which edge the brush reached first - earlier behaviour
  depended on stroke direction.
- **`Shave` and `Raise` change terrain height.** On a path or a foundation that has to stay at
  its level, pick the mode that moves blocks the direction you can afford.
- **Slabs and stairs are not full blocks.** Anything that later tests for a solid surface —
  mob spawning, some redstone, pathing - behaves differently on smoothed terrain.

## Keys

| Action | What it does |
|---|---|
| `Right mouse` | Brush a stroke |
| `Escape` | Cancel the stroke in progress |
