# Settings

DBTools' own options. Opens from **main menu bar → DBTools → Settings**.

There is currently **one** option. Most of what you might look for here lives elsewhere:
per-tool settings are in each tool's panel, and presets are saved per tool rather than
globally.

## The option

### `Show 2nd Fly Speed Slider`

On by default. Adds a second fly speed slider to Axiom's hotbar screen, beside the one Axiom
already provides.

The point is not having two sliders - it is the pair of *stored* speeds. Set one slider to a
survey speed for crossing a build and the other to a detail speed for working inside it, then
flip between them with the **Toggle Fly Speed** keybinding. Each toggle swaps the live speed
with the stored one, so the speed you just left is the one you come back to.

Untick this if the extra slider crowds the hotbar screen; the keybinding keeps working, since
it swaps against a stored value rather than reading the slider.

## Toggle Fly Speed

The keybinding is **unbound by default** - this is the mod's only Minecraft keybinding, and you
have to set it yourself in **Options → Controls → DBTools**.

It is a no-op when the server does not allow changing fly speed, so on a multiplayer server
without the necessary support nothing happens and nothing is reported.

## Where it is stored

`config/dbtools/dbtools.json`, written as soon as you toggle the checkbox. Deleting the file
restores defaults.

The second slider's *speed value* is not stored there - it lives in memory for the session, so
it resets when the game restarts.
