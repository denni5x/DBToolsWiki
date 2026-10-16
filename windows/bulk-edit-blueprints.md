# Bulk Edit Blueprints

Rename, retag and re-author many blueprints in one pass.

Opens from **main menu bar → DBTools → Bulk Edit Blueprints**. This is a
[window, not a tool](../concepts.md#three-surfaces): it edits files in your blueprint
folder, and **nothing it does goes through Axiom's undo**.

## What it does

A blueprint carries a header — name, author, tags, and the yaw/pitch its thumbnail is
rendered from — separately from its blocks. This window rewrites those headers across a
selection, and optionally renames the `.bp` files to match.

It is for the state your blueprint folder reaches after a few months: forty assets named
`blueprint_1` through `blueprint_40`, half with no author, none tagged.

## The workflow

1. **Browse and select.** The blueprint browser on the left walks your blueprint folder;
   `← Back` goes up a level. Tick blueprints to add them to the selection, which persists as
   you navigate between folders. `Selected: n` is the running count, and
   `Clear All Selection` empties it.
2. **Check the preview.** The selected blueprint renders on the right, so you can confirm
   you have the right assets before touching them.
3. **Set the options you want** under `Bulk Edit Options`. Each group has its own checkbox,
   and an unchecked group is left alone.
4. **`Apply Changes`.** A progress bar reports percentage and elapsed time, one blueprint at
   a time.

`Apply Changes` is disabled while an edit is running and while the selection is empty.

## The options that matter

### `Modify Names` — prefix and suffix

Builds the new name as `prefix + existing name + suffix`. `{i}` anywhere in the suffix is
replaced with the blueprint's **1-based position in the selection**, so `_{i}` turns a
selection into `..._1`, `..._2` and so on.

This changes the **name stored in the blueprint**, not the filename. See the gotchas.

### `Search and Replace in Names`

Applied **after** `Modify Names`, to the result. So with both on, a prefix is added first and
is itself a candidate for replacement.

The match is **literal text, not a regular expression**, and every occurrence in the name is
replaced.

### `Also rename physical files`

Nested under `Search and Replace`, and **on by default**. This is the only control that
renames the `.bp` files on disk.

### `Change Author`

Sets one author string across the whole selection. There is no per-blueprint variation.

### `Add Tags:` / `Remove Tags:`

Two tag widgets. `Remove Tags:` offers the tags actually present across your selection, so
you are picking from reality rather than typing blind.

These have **no enabling checkbox** — unlike every other group here, they apply whenever the
widgets hold entries.

### `Change Preview Angle`

Sets `Yaw/Pitch` for the thumbnail across the selection, re-rendering each one. Useful when a
batch of assets was all captured from an unhelpful angle.

## Gotchas

- **There is no undo.** Axiom's undo history covers world edits, not your blueprint folder.
  Back the folder up before a large batch. This is the single most important thing on this
  page.
- **`Modify Names` alone does not rename files.** The filename is only changed when
  `Search and Replace in Names` is on *and* `Also rename physical files` is ticked. Use
  `Modify Names` by itself and a blueprint called `Oak Tree` sits in a file still called
  `blueprint_12.bp`. That is legal and the browser copes, but it makes the folder harder to
  work with outside Minecraft.
- **A rename is a write-then-delete, not a move.** New files are written during the batch, and
  the originals are deleted only once the whole batch finishes. If the game is closed or
  crashes mid-batch you are left with **both** copies, not a corrupt one — recoverable, but
  you have to clear out the originals yourself.
- **Name collisions get a numeric suffix.** If the new filename already exists, the file
  becomes `name_1.bp`, then `name_2.bp`, and so on. Replacing a distinguishing part of a name
  across a selection — every `tree_oak`, `tree_birch` → `tree` — therefore produces
  `tree.bp`, `tree_1.bp`, `tree_2.bp` and loses the distinction. The *stored* names all
  become identical, which the suffix does not fix.
- **Search and replace is literal and global.** No regex, no anchoring, no word boundaries.
  Replacing `o` with `0` hits every `o` in every selected name.
- **The selection survives navigation.** Blueprints ticked in one folder stay selected after
  you navigate elsewhere. Check `Selected: n` before applying, rather than trusting what is
  on screen.
- **Tags apply without a checkbox.** Leave a tag in the `Add Tags:` widget from a previous
  pass and it is applied to the next batch too.
