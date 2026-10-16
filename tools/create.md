# Create

Turn selections into reusable blueprints, in bulk.

## What it's for

Harvesting. You have a world full of builds — a village, a batch of rocks you modelled, forty
variations on a window — and you want them as [blueprints](../glossary.md#blueprint) you can
place elsewhere.

The point is the bulk: selecting forty assets and saving forty blueprints in one pass, named
and numbered, rather than copying each one by hand.

## The loop

1. **Set the destination** with `Set Blueprint Folder`.
2. **Pick a selection `Type`** — `Grid` for an explicit box, `Magic` for contiguous-region
   selection.
3. **Select your assets.** With `Magic`, each click adds a region.
4. **Tune what gets saved** — `Save Air`, `Save Entities`, `Limit per asset`,
   `Min Blocks per Asset`.
5. **`Create Blueprints (Enter)`.**

`Clear (Delete)` empties the selection without leaving the tool.

## The three knobs that matter

### `Type` — Grid or Magic

`Grid` is a box you drag: predictable, and right when the asset has clean bounds. `Magic`
selects contiguous blocks, which is what you want for an organic shape sitting on terrain it
should not include.

### `Min Blocks per Asset`

A threshold below which a selected region is **skipped entirely** rather than saved. This is
the control that makes magic selection practical: a magic pass over a village picks up the
buildings and also every stray torch and flower pot, and this discards the noise instead of
leaving you to delete forty junk blueprints afterwards.

Set it from the smallest thing you genuinely want. If the smallest real asset is a 20-block
bush, 15 is about right.

### `Save Air`

Keeps air blocks inside the asset's bounds. Off, a blueprint carries only solid blocks and
pastes as a stamp that leaves surroundings intact. On, the air is part of the asset and will
clear space when pasted.

On matters for anything with an interior you want preserved — a hollow building, a doorway, a
tunnel mouth. Off is the usual choice for props.

## Gotchas

- **`Min Blocks per Asset` skips silently.** A region below the threshold is not saved and not
  reported, so a pass that produces fewer blueprints than you selected regions is working as
  configured, not failing.
- **`Digits` only pads the numeric suffix.** `Set number of suffix digits` controls zero
  padding (`asset_001` rather than `asset_1`), which matters because most tools sort names as
  text — without padding, `asset_10` sorts before `asset_2`.
- **`Save Entities` is separate from `Save Air`.** Item frames, armour stands and paintings are
  entities, not blocks; a blueprint of a decorated room without this on comes back bare.
- **`Use Relative Position`** stores the asset's offset from the selection origin rather than
  normalising it, which matters when you intend to paste a set of blueprints back in formation.
- **The output folder is remembered, not per-run.** Check it before a large pass; it is easy to
  dump forty assets into the folder you used last week.

## Keys

| Action | What it does |
|---|---|
| `Left mouse` / `Right mouse` | Build the selection |
| `Enter` | Create the blueprints |
| `Delete` | Clear the selection |
| `Escape` | Cancel |
| `Undo` | Step back a selection |
| `Copy` / `Cut` | Axiom's own, on the current selection |
| `Scroll` | Adjust the selection |
