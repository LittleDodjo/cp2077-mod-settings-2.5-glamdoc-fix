# Mod Settings: Glamdoc fix for Cyberpunk 2077 2.5

Rebuilt `ModSettings.archive` for [Mod Settings](https://www.nexusmods.com/cyberpunk2077/mods/4885)
on game patch 2.5 (3.0.88.46235). Fixes the empty Glamdoc screen.

## The problem

Mod Settings ships an archive that replaces two game files to add its `mod_settings_main` entry:

- `base\gameplay\gui\fullscreen\menu.inkmenu`
- `base\gameplay\gui\fullscreen\main_menu\pregame_menu.inkmenu`

The copies inside the original archive come from game 2.3. On 2.5 they are missing every menu
added since: `glamdoc`, `controller_switch2`, `modio_browser_menu`, `pitch_menu`,
`pitch_telemetry_menu`, `pitch_eula`. When you talk to a Glamdoc (the new cyberware style vendor,
spray-can icon on the map), the vendor hub switches to the `glamdoc` menu, the menu is not in the
registry, nothing spawns, and you get an empty screen with only your eddies in the corner.

This is **not** caused by Cyberware-EX, ArchiveXL, Codeware, TweakXL, redscript or your save. The
"unofficial 2.50 build" of Mod Settings has the same problem: it rebuilt the DLL and kept the old archive.

How to recognise it: the Cyberware tab in the hub menu works, regular ripperdocs work, only the
Glamdoc is blank, and the blank screen has no tabs and no level/street cred on the left.

## The fix

Both `.inkmenu` files are the 2.5 originals with the `mod_settings_main` entry inserted after
`settings_main`, exactly as before. Nothing else in the archive changes.

The archive is taken from [jackhumbert/mod_settings#34](https://github.com/jackhumbert/mod_settings/pull/34)
by [butrico31](https://github.com/butrico31) (commit `f13e57dac850`). This repository only
redistributes it so that it can be linked from Nexus until the pull request is merged and released.

## Install

Copy `red4ext\plugins\mod_settings\ModSettings.archive` into the game folder, replacing the existing
file:

```
<Cyberpunk 2077>\red4ext\plugins\mod_settings\ModSettings.archive
```

Or unpack the release zip into the game folder. Requires the 2.5 builds of RED4ext and Mod Settings.

SHA-256 of the archive: `deeed626e73aeb911fdc9da1f4d39fcddef532ec6dd8d1f36d8eaf3a4b3c4c59`

Alternative without patching: delete `ModSettings.archive`. The Glamdoc works again, but the
Mod Settings entry disappears from the pause menu.

## Verified

On 2.5 (3.0.88.46235) with RED4ext 1.31.0, redscript 0.5.32, Codeware 1.21.0, TweakXL 1.12.0,
ArchiveXL 1.28.0, CET 1.38.0, Cyberware-EX 1.6.0: the Glamdoc lists its styles, regular ripperdocs
and the hub Cyberware tab work as before.

Since these files are full copies of game files, they will need the same refresh after any future
patch that adds menus.

## License

Mod Settings is MIT licensed (see `LICENSE`, copyright Jack Humbert). The rebuilt archive is
redistributed under the same terms.
