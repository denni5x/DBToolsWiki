# Prototype

Block out massing from blueprints before committing detail.

## What it's for

Deciding where things go before deciding what they look like. The tool generates
**placeholder structures** - towers, houses, modular houses, bridges, fences, build assets —
from your blueprints, so you can lay out a settlement's volumes and silhouettes and judge the
composition before building anything properly.

It is the only tool here aimed at the stage *before* detailing, which is why it has a
clipboard of its own and gizmos rather than a brush.

## The loop

1. **`Load from Blueprint Browser`** to give the tool the parts it will assemble from.
2. **Pick a mode** under `Modes` - tower, house, modular house, bridge, fence, build asset.
3. **Drag out the structure** in the world, with `Display Gizmos` on so the handles are
   visible.
4. **Adjust** under `Object Settings` - each mode has its own, and a `Theme` with `Default` and
   `Variant` options.
5. **`Enter`** to commit, or keep going and build up the massing.

`Clear Prototype Clipboard` resets the tool's own clipboard.

## The three knobs that matter

### `Render Opacity`

Draws the prototype semi-transparent. This is the control that makes the tool work as a
*blockout*: at partial opacity you read the massing and the sight lines rather than the
blocks, and you can see the terrain and neighbouring structures through it.

Push it down while composing, back up when judging a specific structure.

### `Display Gizmos`

Shows the drag handles. On while you are shaping something; off when you want to look at the
result without handles in the way. Toggling this is the fastest way to evaluate what you have
without committing.

### `Auto Texturing` (with `Hollow All`)

`Auto Texturing` assigns blocks to the generated structure from the theme rather than leaving
it a single material - the difference between a grey mass and something you can judge as a
building.

`Hollow All` makes the generated volumes shells rather than solids, which both reads better
and costs far fewer blocks.

## Gotchas

- **Prototypes are real blocks once committed.** Nothing stays a placeholder - `Enter` writes
  actual blocks. The workflow expects you to delete the blockout and build properly, or
  overwrite it in place.
- **`Hollow All` off on a large tower is a lot of blocks.** A solid volume is filled
  completely; the blockout stage rarely needs that.
- **Each mode has its own `Object Settings`.** Switching mode changes the entire lower panel,
  so settings you tuned for a tower do not carry to a bridge.
- **It has its own clipboard**, separate from Axiom's. `Clear Prototype Clipboard` affects the
  tool's, not Axiom's, and copying in Axiom does not populate this one - use
  `Load from Blueprint Browser`.
- **`Use Relative Position`** keeps each part's stored offset, which matters when the
  blueprints you loaded were authored as a matched set.

## Keys

The busiest tool in DBTools for key actions.

| Action | What it does |
|---|---|
| `Left mouse` / `Right mouse` | Drag out and position structures |
| `North` / `South` / `East` / `West` / `Up` / `Down` | Nudge the placement |
| `Rotate Placement` / `Flip Placement` | Reorient |
| `Scroll` | Adjust the current handle |
| `Copy` / `Paste` | The tool's own prototype clipboard |
| `Undo` / `Redo` | Step through prototype edits |
| `Enter` | Commit |
| `Escape` | Cancel the placement |

It is also the only tool with a number key of its own: **`9`** selects it.
