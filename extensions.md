# Extensions

Controls DBTools adds to screens that belong to **Axiom**, not to DBTools.

This is the third of the mod's [three surfaces](concepts.md#three-surfaces), and the hardest
to discover: these appear with no DBTools branding around them, in screens you already know,
so there is nothing to tell you they are not Axiom's own. If you have ever wondered where a
feature you read about lives, it is probably here.

There are three.

## Export Palette - creative colour picker

Axiom's creative colour picker lists ten blocks matching the colour you picked. This adds a
**side tab below Axiom's own**, marked with a painting icon and labelled
`Export Colours as Palette`, that turns those ten blocks into a new palette in Axiom's
Palettes panel.

The palette is named after the colour in hex - `Colour 7F3C1A` - so repeated exports stay
apart rather than overwriting one another. (No `#`, because Axiom rejects it in a name.)

The button's own tooltip reports the result: `Exported as "..."` on success, or
`Could not create the palette`. Since your cursor is still on the button when you click it,
you see the outcome immediately without looking anywhere else.

**Where the tab sits** depends on whether Axiom's gradient helper is enabled, because this tab
takes whichever slot is free below Axiom's. Turn the gradient helper on or off and this button
moves.

## Export Palette - gradient helper

The same idea in Axiom's gradient helper, labelled `Export Gradient as Palette` and sitting
directly below the helper's own tab.

The important detail: it exports the **generated outputs**, not the inputs you chose. The
gradient screen holds your input blocks in the first nine slots and the generated ramp in the
next nine, and this takes the ramp. So the workflow is: build a gradient you like, then export
it, and you have the whole ramp as a palette to paint with.

Palettes from here are named `Gradient`, with ` (2)`, ` (3)` and so on appended as you export
more.

## Second fly speed slider

Axiom's hotbar screen has a fly speed slider. This adds a **second one beside it**, holding an
independent stored speed, so you can keep two speeds set up and flip between them with the
**Toggle Fly Speed** keybinding instead of dragging a slider every time.

The intended use is one survey speed for crossing a build and one detail speed for working
inside it. Each toggle *swaps* the live speed with the stored one, so the speed you just left
is the one you come back to - it is a flip, not a jump to a fixed value.

Controls on the second slider match Axiom's:

| Input | Effect |
|---|---|
| Drag | Set the speed directly |
| Left click the arrows | Fine step |
| Right click the arrows | Coarser step |
| Hold an arrow | Repeats after a short delay |

**Dragging covers 1× to 10×**, and the response is deliberately non-linear - curved so that
most of the slider's travel covers the low speeds, where the difference between 1× and 2×
matters, rather than spending half the bar on speeds you cannot steer at. Half the travel gets
you to roughly 3×, not 5×.

The arrow buttons are not limited to that range: they step down as far as **0.01×**, well
below anything drag can reach. That is the control to use for creeping along a surface - but
it also means a slider that looks empty may be set to a crawl rather than to its minimum
useful speed.

It is toggled by `Show 2nd Fly Speed Slider` in [Settings](windows/settings.md). It sits on
whichever side your main hand is set to, mirroring with Axiom's.

## Gotchas

- **Fly speed needs server support.** Both the slider and the keybinding depend on the server
  supporting a fly speed change. Without it the slider still draws but **cannot be adjusted**,
  and the keybinding does nothing - with no error either way. On a server where it seems
  broken, this is almost always why.
- **The second speed is not saved.** `Show 2nd Fly Speed Slider` is stored in
  `config/dbtools/dbtools.json`, but the speed itself lives in memory for the session and
  returns to its default (2×) when the game restarts.
- **Toggle Fly Speed is unbound by default.** It is the mod's only Minecraft keybinding, and it
  does nothing until you set it in **Options → Controls → DBTools**.
- **An export with nothing to export fails quietly-ish.** If the slots hold no usable blocks,
  no palette is created and the button says `Could not create the palette` - it is reported in
  the tooltip only, so it is easy to miss if you move the mouse away immediately.
- **Duplicate palette names get a counter, up to 99.** Axiom refuses a duplicate name, so
  ` (2)` through ` (99)` are tried; past that the export fails. Only reachable by exporting the
  same colour a hundred times, but that is the limit.
- **Exported palettes are Axiom's now.** They live in Axiom's Palettes panel and are managed
  there - renaming or deleting one is an Axiom operation, and DBTools has no further
  involvement.
- **The colour picker's tab moves.** Because it takes the free slot below Axiom's, enabling the
  gradient helper shifts it down. Muscle memory for its position does not survive that change.

## Not on this page

DBTools also hooks several Axiom internals that add **no controls** - the toolbar icon
rendering that puts the twelve tools in Axiom's tool window, the icon font merge that gives
them glyphs, and a world render hook. These are plumbing: there is nothing to configure and
nothing to find.

The [Node Editor](windows/node-editor.md) is not here either. It is a window on the
[DBTools menu](README.md#2-windows--the-dbtools-menu), not an extension of an Axiom screen.
