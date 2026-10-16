# File Conversion

Convert between Axiom blueprints, schematics, and Hytale prefabs.

Opens from **main menu bar → DBTools → File Conversion**. A
[window, not a tool](../concepts.md#three-surfaces): it reads and writes files in bulk, and
**nothing it does goes through Axiom's undo**.

Three tabs, one per direction.

## Blueprint to Schematic

Converts **every `.bp` in the chosen folder** into a schematic.

### The workflow

1. `Blueprint` → `Set Folder`: the folder to read. Defaults to Axiom's blueprint directory.
2. `Schematic` → `Set Folder`: where the schematics are written.
3. Optionally pick a `Version` to target.
4. `Convert Blueprints to Schematics`. A progress popup reports percentage and elapsed time,
   with `Cancel` while running and `Close` when done.

### The options that matter

**`Version`** targets an older Minecraft version. This is an **Axiom Commercial License
feature** — without one the dropdown is disabled, with a tooltip saying so, and conversion
uses the current version.

The **file extension follows the target version** and is not something you choose: a target
whose max data version is below 1631 gets `.schematic`, everything else gets `.schem`.

## Schematic to Blueprint

The reverse, reading `.schem`, `.schematic` and `.litematic`.

### The workflow

1. Set the source and destination folders.
2. Decide on `Save Air`.
3. `Convert Schematics to Blueprints`.

### The option that matters

**`Save Air`** keeps air blocks in the resulting blueprint rather than dropping them. Keep it
on when the schematic's empty space is load-bearing — a hollow interior, a doorway, a
clearance volume you intend to paste over existing blocks. Leave it off and the blueprint
only carries the solid blocks, which is smaller and usually what you want for an asset.

## Blueprint/Schem to Hytale Prefab

Converts a blueprint or schematic into a Hytale prefab (a JSON file), through a block mapping
you can edit.

### The workflow

1. `Set Source Folder`, then pick `Source Type:` — `Blueprint (.bp)` or
   `Schematic (.schem/.litematic)`. One or the other, not both in a pass.
2. **Block Mapping.** Leave `Use Default Mapping` on to use the mapping shipped with DBTools,
   or untick it and `Select Custom Mapping File` to supply your own `.txt`.
3. `Set Output Folder` — **required**; `Convert to Hytale Prefab` stays disabled until one is
   chosen.
4. Pick an `Anchor Mode`.
5. `Convert to Hytale Prefab`.

### The options that matter

**`Export Default Mapping`** writes the built-in mapping out as `hytale_block_mapping.txt`.
This is the intended way to make a custom mapping: export, edit the lines you care about,
then load it back with `Select Custom Mapping File`. Writing one from scratch is not worth
it.

**`Anchor Mode`** decides the prefab's origin. `Center (region center)` uses the horizontal
centre of the region at its minimum Y — the bottom-middle, which is what you want for
something that sits on the ground. `Custom (specify below)` reveals a
`Custom Anchor (X,Y,Z)` field and anchors every prefab in the pass at those coordinates.

## Gotchas

- **No undo, and existing files are overwritten.** Convert into a folder twice and the second
  pass replaces the first. Convert into a fresh folder when you are not sure.
- **Folder scans are not recursive.** Only files directly in the chosen folder are converted;
  subfolders are ignored and reported as nothing to do rather than as an error.
- **A custom anchor is one point for the whole batch.** It is absolute coordinates, not an
  offset, so converting a folder with `Custom` anchors every prefab in it at the same spot.
  Use `Center` for a batch of unrelated assets.
- **`Version` silently falls back.** Without a Commercial License the dropdown is disabled and
  conversion proceeds at the current version rather than refusing — so a batch you expected to
  be 1.20.1 will be current-version schematics with no error.
- **`Reload Mapping` is debug-only.** It only appears when DBTools is in debug mode. In a normal
  session, a mapping file edited on disk is picked up by re-selecting it, not by reloading.
- **The extension is chosen for you.** You cannot ask for `.schem` while targeting an old
  version, or vice versa; it follows the target version's data version.
- **One source type per pass.** The Hytale tab reads either blueprints or schematics, so a
  mixed folder needs two passes.

