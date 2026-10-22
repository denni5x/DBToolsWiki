# BP→Path

Scatter blueprints along a curve, oriented to it.

## What it's for

Anything that follows a line: fence posts along a boundary, lamps down a street, trees beside a
river, sleepers along a railway, rocks edging a path.

The alternative is placing each one by hand and rotating it to match the curve, which is both
slow and visibly imperfect. This draws the curve once and distributes along it.

## The loop

1. **Add your blueprints** - `Add Blueprint` from the browser, or `Add Clipboard` for what you
   just copied. Several can be added, and `Paste Mode` decides how the tool picks between them.
2. **Draw the curve** by placing points in the world. Points are draggable after the fact.
3. **Set `Distance`** - the spacing between placements.
4. **Decide on `Orient To Path`** and `Roll`.
5. **`Paste Blueprints`.**

Or load a [preset](../glossary.md#preset) from `Presets`, which carries the settings and the
blueprint set - see [Presets](#presets) below.

## The three knobs that matter

### `Distance`

Spacing along the curve, and the control you will spend the most time on. Too tight and
assets interpenetrate; too loose and the line reads as dots rather than a boundary.

It is measured along the curve, so a tight bend packs placements closer in straight-line terms
than the number suggests.

### `Orient To Path`

Rotates each placement to follow the curve's direction. Essential for anything with a front —
a bench, a signpost, a carriage. Turn it off for radially symmetric assets like rocks and
bushes, where orienting them adds nothing and makes the repetition more obvious.

### `Roll`

Rotates each placement around the path's own axis. Its use is terrain-following: on a path
that climbs, `Roll` lets assets bank with the slope rather than staying stubbornly level.

`Unlock Rotation` frees the rotation axes from each other when you need an orientation the
combined controls will not otherwise reach.

## Breaking up the regularity

A run of identical blueprints at identical spacing reads as a fence even when it is meant to be
trees. Three controls exist to break that up.

### `Random Spacing`

Turns `Distance` into a min/max range, and each blueprint picks its own gap inside it. The
widget carries both numbers: the half the cursor is over picks the one the scroll wheel moves,
left for the minimum and right for the maximum, matching the drag handles. Nudging one end past
the other clamps rather than inverting the range.

### `Paste Mode`

**`Chance Based`** (the default) rolls a weighted pick per placement. **`Alternating`** steps
through the set in order, so three blueprints give 1, 2, 3, 1, 2, 3 - which is regular, and
sometimes exactly what you want for paving or sleepers.

### `Random Yaw` and `Random XZ Flip`

Free rotation and mirroring per placement. Worth having on for anything radially vague - rocks,
bushes, rubble - and off for anything with a front.

## Sitting on the terrain

Two different answers to a path drawn across uneven ground:

- **`Extend To Ground`** stretches each blueprint downward until it meets the surface. Right for
  anything that should look founded: a wall, a pillar, a building.
- **`Clip To Ground`** moves each blueprint down *or up* onto the ground under it, without
  stretching. Right for anything that sits on the surface rather than growing out of it: trees,
  rocks, props. This is what lets a path drawn roughly level still land correctly on terrain that
  is not.

## `Place` - immediate or deferred

**`Immediate`** pastes the blocks when you commit.

**`Deferred`** turns each blueprint into a placement of its own, with a gizmo to move and turn
it by, and waits for a second confirm. Use it when the distribution is nearly right and three or
four placements need nudging by hand - which is most paths, in practice.

The sixty-four placements nearest the camera get gizmos, matching Axiom's own Stamp tool. Undoing
a deferred paste hands back the arranging as well as the pasting: the placements come back where
you left them, nudges and all.

## Presets

`Presets` saves the settings **and** the blueprint set, reloaded in one click.

Blueprints are stored as references to their files rather than as blocks, so a preset stays a few
hundred bytes and a shared one resolves against the other machine's library. A blueprint that
cannot be found comes back as a row saying "file missing" that offers to be pointed at one again,
rather than quietly placing less than it used to.

**The points are not stored.** A preset is a path's settings; a path belongs to the place it was
drawn.

## Gotchas

- **`Alternating` is regular by design.** Add three blueprints in that mode and you get 1, 2,
  3, 1, 2, 3 along the path, which is visible on a long run. `Chance Based` is the default for
  that reason.
- **`Distance` is arc length.** On a curve with a tight radius, placements that are evenly
  spaced along the path look bunched from above.
- **A preset does not restore your path.** Loading one gives you the settings and the blueprints;
  the points are yours to draw.
- **Spacing does not know about asset size.** Unlike the Biome Brush's `Min Spacing`, this is
  an absolute distance, so swapping a small blueprint for a large one will overlap at the same
  setting.
- **Escape does not cancel a drawn curve the way it cancels other placements** - this tool
  reports a *deselect point* action instead, so Escape steps out of the active point rather
  than discarding your path.

## Keys

| Action | What it does |
|---|---|
| `Left mouse` | Place or drag a curve point |
| `Right mouse` | Begin / adjust the path |
| `Extrude` | Extend the path |
| `Enter` | Paste the blueprints |

## Related

- [Concepts → Presets](../concepts.md#presets)
- [Biome Brush](biome-brush.md) - weighted scattering over an area rather than along a line
- [River](river-tool.md) - the other points-and-a-curve tool, which also moves the terrain
