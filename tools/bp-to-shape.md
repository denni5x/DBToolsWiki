# BP→Shape

Scatter blueprints around a shape, by count.

## What it's for

Regular arrangements: pillars around a circular hall, torches around a plaza, standing stones
in a ring, a spiral of lanterns up a tower.

The difference from [BP→Path](bp-to-path.md) is how you specify density. A path takes a
`Distance` and fits as many as will fit; a shape takes a **`Blueprint Count`** and spaces them
evenly. When the requirement is "twelve pillars, evenly spaced", this is the tool.

## The loop

1. **Add your blueprints** — `Add Blueprint` or `Add Clipboard`.
2. **Pick a shape**: disk, plane, regular polygon, superellipse, or Archimedean spiral.
3. **Size it** — `Size X` / `Size Z`, or `Size` with `Unlock XZ Size` off for a uniform shape.
4. **Set `Blueprint Count`** and check `Total Count`.
5. **Decide on `Orient To Shape`** and `Place on Corners`.
6. **`Paste Blueprints`.**

## The three knobs that matter

### `Blueprint Count` (and `Total Count`)

How many placements, distributed evenly around the shape. `Total Count` reports what you will
actually get, which is not always the number you typed — corners and shape closure affect it.
Read `Total Count` before pasting.

### Shape type

Five shapes, and the choice does more work than it looks like:

| Shape | Use |
|---|---|
| Disk | A ring — the common case |
| Plane | A rectangular boundary |
| Regular polygon | A ring with flat sides, for architecture that should read as built |
| Superellipse | Between a circle and a rectangle — rounded-corner plazas |
| Archimedean spiral | The only open shape here; everything else closes |

### `Place on Corners`

On an angular shape, forces a placement at each vertex. This is the difference between a
polygon that reads as deliberate and one that looks like a circle someone got wrong — the
corners are the shape's defining feature, and even spacing alone will not land on them.

Irrelevant for a disk.

## Gotchas

- **The spiral does not close**, so counts behave differently on it: there is no wrap-around,
  and the last placement is at the outer end rather than adjacent to the first.
- **`Total Count` is the truth.** `Blueprint Count` is a request; corner placement and shape
  closure adjust it.
- **Several blueprints alternate rather than randomise**, same as BP→Path — on a twelve-point
  ring with three blueprints you get a visible 3-fold pattern. That is often exactly what you
  want for architecture, and exactly what you do not want for rocks.
- **`Size` is the shape's extent, not the assets'.** Twelve large pillars on a small radius
  will interpenetrate; the tool does not check.
- **`Orient To Shape` turns assets to face the shape**, which for an inward-facing ring is
  right and for a row of identical rocks is wasted. `Unlock Rotation` is there for the cases
  the combined controls cannot express.

## Keys

| Action | What it does |
|---|---|
| `Right mouse` | Begin / position the shape |
| `Enter` | Paste the blueprints |
| `Escape` | Cancel the shape placement |
| `Delete` | Clear the placement |
