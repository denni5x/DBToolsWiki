# Overview

<div align="center"><img src=".gitbook/assets/icon.png" alt="DBTools Icon" height="128" width="128"></div>

## Support/ Contact

&#x20;&#x20;

&#x20;&#x20;

An [Axiom](https://modrinth.com/mod/axiom) add-on for Minecraft: eleven world-building tools, a node editor, bulk blueprint editing, and file conversion.

## Start here

* [**Concepts**](concepts.md) - how DBTools sits on Axiom, its [three surfaces](concepts.md#three-surfaces), and the ideas every tool shares: preview-then-commit, masks, layers, coverage, presets, keys.
* [**Glossary**](glossary.md) - the mod's invented vocabulary, defined once.

If a tool page confuses you, the term you are missing is almost certainly in one of those two. If you cannot _find_ a feature, it is on one of the three surfaces below - most often the menu bar.

## 1. Tools - Axiom's toolbar

In toolbar order. Select one and it takes your clicks.

| Tool                                | What it's for                                                      |
| ----------------------------------- | ------------------------------------------------------------------ |
| [Create](tools/create.md)           | Turn a selection into reusable blueprints                          |
| [Place](tools/place.md)             | Place a blueprint with a live, surface-aware preview               |
| [Rotate](tools/rotate.md)           | Rotate a pasted blueprint to any angle, snapped or free            |
| [BP→Path](tools/bp-to-path.md)      | Scatter blueprints along a curve, oriented to it                   |
| [BP→Shape](tools/bp-to-shape.md)    | Scatter blueprints around a shape, by count                        |
| [Grid Place](tools/grid-place.md)   | Tile a blueprint across a grid                                     |
| [Tree](tools/tree.md)               | A parametric tree, every parameter live                            |
| [Prototype](tools/prototype.md)     | Block out massing from blueprints before committing detail         |
| [OBJ to MC](tools/obj-to-mc.md)     | Turn an OBJ, glTF/GLB, STL or PLY model into colour-matched blocks |
| [Slab Brush](tools/slab-brush.md)   | Smooth terraced terrain into slabs and stairs                      |
| [Biome Brush](tools/biome-brush.md) | Paint an area, stack layers of blocks and objects on it            |

## 2. Windows - the DBTools menu

On Axiom's main menu bar. These operate on **files and folders**, not on the world: no preview, no Enter-to-commit, and **no undo step**. Back up a blueprint folder before a bulk change.

| Window                                                      | What it does                                                                                                                                               |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [**Bulk Edit Blueprints**](windows/bulk-edit-blueprints.md) | Search-and-replace in names (optionally renaming the files too), change author, add and remove tags, change preview angle - across many blueprints at once |
| [**File Conversion**](windows/file-conversion.md)           | Three tabs: Blueprint → Schematic, Schematic → Blueprint, and Blueprint/Schem → Hytale Prefab, with a configurable block mapping                           |
| [**Node Editor**](windows/node-editor.md)                   | A node graph for terrain and biomes; opening it deselects Axiom's active tool                                                                              |
| [**Settings**](windows/settings.md)                         | DBTools' own options (currently just `Show 2nd Fly Speed Slider`)                                                                                          |

## 3. [Extensions](inside-axiom/extensions.md) - inside Axiom's own UI

Controls DBTools adds to screens that belong to Axiom, so they carry no DBTools branding — the hardest part of the mod to discover. Full detail in [**Extensions**](inside-axiom/extensions.md).

| Extension               | Where                                                                                                            |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Export Palette          | Axiom's creative colour picker, and its gradient helper - a side tab under Axiom's own                           |
| Second fly speed slider | Axiom's hotbar screen, next to the first; toggled in Settings                                                    |
| Toggle Fly Speed        | The mod's one Minecraft keybinding, **unbound by default** - swaps live fly speed with the second slider's value |

## Elsewhere

* [**Modrinth**](https://modrinth.com/mod/dbtools) - downloads and version support
* [**Changelog**](https://modrinth.com/mod/dbtools/changelog) - what each version added, in detail
* [**Discord**](https://discord.com/invite/ZfVP5RmQzb) - questions and bug reports
* [**YouTube**](https://www.youtube.com/@dennisbuilds) - builds and demos
