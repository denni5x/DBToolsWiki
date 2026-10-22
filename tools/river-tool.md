# River

Draw a course as a line of points, and the tool carves the channel, shapes the banks, fills the
water and scatters blueprints along it.

Terms used here are defined in the [glossary](../glossary.md); the ideas behind them are in
[Concepts](../concepts.md).

## What it's for

Running water through terrain that already has its shape, and the dry version of the same cut.
A river, a stream, a canal, and with `Dry Bed` on, a gully or a canyon.

It is the one DBTools tool that moves terrain rather than dressing it. The
[Biome Brush](biome-brush.md) writes blocks at an existing surface and leaves the surface where
it is; this raises and lowers columns to make room for water, then dresses what it cut.

Doing it by hand is three passes that have to agree with each other: cut the trench, build the
banks back up, fill the water to one level. Miss the agreement anywhere and you get water
standing proud of the ground on one side, or a bank with the trench's dirt on top of it.

## The loop

1. **Select the tool** and place points with right mouse. Two is enough to preview.
2. **Pick a `Height Mode`** on the Water tab. This is the decision everything else sits on, so
   make it first - see below.
3. **Set `Width` and `Depth`** on Channel.
4. **Tune.** Drag anything and the preview re-solves. Nothing is written yet.
5. **Commit with Enter.**

Or load one of the two shipped [presets](../glossary.md#preset), `River Example` and
`Canyon Example`, and start from a river rather than from the defaults.

## The panel

A row of icon tabs rather than a column of sections. Each tab holds the blocks it decides
alongside the shape it decides, so the bank's material lives with the bank's thickness rather
than in a materials section of its own.

| Tab | What it decides |
|---|---|
| `Water` | Whether there is water at all, where its surface sits, and how it steps down |
| `Course` | The curve through the points: `Line`, Catmull-Rom or Bézier, and `Looped` |
| `Terrain Limits` | How far any column may move, and the substrate under what gets built up |
| `Channel` | `Width`, `Depth`, the bed profile and the bed's blocks |
| `Banks` | `Bank Width`, `Bank Height`, the slope curve, the blend, and the bank's blocks |
| `Variation` | The one noise field, and how much of it reaches each of four things |
| `Points` | Every point as a row, with its per-point overrides |
| `Blueprints` | Three independent scatter groups |

Water leads deliberately: whether there is any is the first thing to decide, because with
`Dry Bed` on this is a canyon tool and that is a different thing to be reaching for.

## The knobs that matter

### `Height Mode` - the one to decide first

**`Exact Y`** puts the water surface at the Y each point was placed at, and builds terrain up to
meet it. You are placing the waterline by hand, and the ground follows.

**`Drop To Ground`** drops each point onto the ground beneath it, so terrain is only ever cut
away, never raised. Use it when the river should follow the valley you already have.

Everything downstream of this reads differently depending on which you picked, which is why it
is step 2 and not step 5.

### `Water Surface` - continuous or pooled

**`Continuous`** interpolates the surface along the path. It is where a new river starts and it
is what a flowing stream wants.

**`Level Pools`** holds each stretch flat and steps down between them, which is how still water
actually sits. It has a `Pools Per Point` of its own: each stretch between two points is cut
into that many flat pools, stepping down in turn. Turn it up when a river covers a lot of
height, so it descends in several small steps rather than one tall one.

The failure to watch for is a step taller than the channel is deep. That leaves the upstream
pool's bed standing over the next pool's surface, so the two hold water that never meets.
`Pools Per Point` is the fix, and `Pool Waterfalls` is the other one - it carries the water over
each step as a line of it falling straight down into the pool below, which joins the two however
tall the step is.

### `Flow Downhill` - water does not climb

Stops the surface ever rising along the path, with `Max Drop Per Block` to say how fast it may
fall. A river crossing a rise then carves through it instead of climbing over it.

### `Width` and `Depth`, and the bed profile

`Bed Profile` is picked from drawings of the four silhouettes rather than from a dropdown: flat,
V, U or parabola, biased toward the centre or the edge by `Profile Exponent`. There is always at
least a block of water at the very edge, so a river has shallows rather than a wall of water at
its rim.

### `Max Raise` and `Max Lower` - the safety rails

Cap how far any column may move. They default to their maximum, so a first river lands where you
drew it rather than part way to it; turn them down when the river should respect the terrain more
than it respects your line.

`Skip When Clamped` leaves a clamped column alone entirely rather than moving it as far as
allowed, which is usually what you want - a column moved to its limit and no further is a
visible flat spot.

`Contain Water` overrides `Max Raise` where it has to: it raises the banks to the water line
wherever a limit would otherwise leave them below it. Without it, a wide river crossing a slope
has water standing proud of the ground on the low side.

## Blocks

Bed, bank, shore and substrate are each a noise-painted mix - the same weighted-blocks-by-noise
primitive the [Biome Brush](biome-brush.md)'s terrain layers use.

Each arrives with two blocks in it adding up to the whole mix: gravel and sand on the bed, grass
and coarse dirt on the banks, sand and clay on the shore, stone and dirt for the substrate. So
the panel says what the river is made of as soon as you open it.

A mix paints the part of its noise its percentages add up to and leaves the rest alone, which is
how a river gets carved as a shape rather than as a restyling - empty the mixes and the tool
only moves ground. The exception is ground the river has just raised: there is nothing there to
leave alone, so those blocks take the substrate instead of staying as air.

`Substrate` fills a column under the bands when it has to be built up. By default it copies the
block already under that column's surface so the fill matches local geology; give it a mix of
its own when you want something else.

## Variation

One noise field drives all of it, with a strength per thing it may wander: width, depth, bank
width and bank height. `Smooth`, `Fractal` or `Random`.

The four are drawn from the same field at offsets rather than seeded separately, so they share a
character without moving in lockstep. That matters more than it sounds: a waterline that is an
exact offset of the curve reads as a canal, and four independently seeded wobbles read as noise
rather than as a river.

## Per-point overrides

`Width`, `Depth`, `Bank Width`, `Bank Height`, `Bank Slope` and height are overridable per point,
in three groups - Channel, Banks and Height - and interpolate between the points that override
them.

Grouped rather than per parameter because the parameters inside a group are tuned together. The
use is a river that changes over its length: widen it toward the sea by overriding only the last
point.

A point that overrides nothing resolves to the globals, so moving a global slider still moves
every point you have not spoken for.

## Blueprints

Three scatter groups that share nothing:

- **`Underwater (Bed)`** - on the channel floor, under the water
- **`In Water (Shoreline)`** - standing in the shallows, just inside the waterline
- **`On Land (Shoreline)`** - on dry ground, just outside it

Each is the Stamp tool's scatter: a weighted blueprint set, `Min Spacing` measured in the
object's own footprint, random yaw and flip, a vertical offset, a density, its own
[mask](../glossary.md#mask) and its own presets. Each blueprint in the set also carries an X, Y,
Z offset of its own - see
[Biome Brush → a blueprint's own offset](biome-brush.md#a-blueprints-own-offset).

## Gotchas

- **`Height Mode` changes what every other number means.** `Exact Y` builds ground up to your
  line; `Drop To Ground` only ever cuts. Decide it before you tune anything.
- **A tall step between pools holds water that never meets.** A step deeper than the channel
  leaves the upstream bed above the next pool's surface. Raise `Pools Per Point`, or turn on
  `Pool Waterfalls`.
- **`Smooth Water Surface` is an editor-time appearance.** It slopes the top layer with flowing
  water levels instead of stepping it, and Minecraft re-levels those as soon as the chunk ticks.
  The panel says so next to the toggle.
- **The blend band moves height, not blocks.** The bank's material stops at `Bank Width` rather
  than reaching across the whole `Blend Width`, and a column the blend eases upward keeps its own
  surface block on top instead of being capped with the dirt from under it.
- **Plants are cleared from ground the river reshapes.** Grass, tall grass, ferns, flowers and
  snow go with the ground they stood on, two-block plants whole. A column the river reaches over
  without changing keeps its plants, since nothing moved under them.
- **Presets do not store the points.** A preset is a course's settings and a course belongs to
  the place it was drawn.
- **`Dry Bed` makes this a different tool.** No water, just the cut - which with `Clear Above
  Water` for headroom, or `Clear To Sky`, is a gully and canyon tool rather than a river one.

## Keys

Actions this tool claims, in Axiom's terms (see [Keys](../concepts.md#keys)):

| Action | What it does |
|---|---|
| `Right mouse` | Add a point to the course |
| `Left mouse` | Select or drag a point |
| `Extrude` | Extrude the selected point along an axis |
| `Enter` | Commit the river (needs two points) |
| `Delete` | Delete the selected point, or clear the whole course if none is selected |
| `Escape` | Deselect the active point |

Ctrl / Cmd with left mouse keeps the current selection rather than clearing it.

## Related

- [Concepts → Preview, then commit](../concepts.md#preview-then-commit)
- [Concepts → Masks](../concepts.md#masks) - the scatter groups' masks
- [Concepts → Presets](../concepts.md#presets)
- [Biome Brush](biome-brush.md) - dressing a surface rather than moving it
- [BP→Path](bp-to-path.md) - the other points-and-a-curve tool
