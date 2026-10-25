**Blank Glamdoc screen on 2.5 is not caused by Cyberware-EX, it's Mod Settings**

If you updated to 2.5, installed the latest Cyberware-EX (1.6.0) with the 2.5 builds of RED4ext, redscript 0.5.32, Codeware 1.21.0, TweakXL 1.12.0 and ArchiveXL 1.28.0, and the new Glamdoc (cyberware style vendor, spray-can icon on the map) opens as an empty screen with only your eddies in the corner, Cyberware-EX is not the culprit.

I spent a few hours bisecting this: removed Cyberware-EX, then every redscript and CET mod, then every archive mod that replaces vanilla files. The screen stayed blank every time. The hub menu controller correctly detects the NPC as a Glamdoc and switches to the `glamdoc` menu, but the menu never gets created, so the hub shows an empty frame.

**The actual cause is Mod Settings.** Its `ModSettings.archive` replaces two game files to add its own menu entry:

- `base\gameplay\gui\fullscreen\menu.inkmenu`
- `base\gameplay\gui\fullscreen\main_menu\pregame_menu.inkmenu`

The copies inside the archive come from game 2.3, so on 2.5 they are missing every menu added since: `glamdoc`, `controller_switch2`, `modio_browser_menu`, `pitch_menu`, `pitch_telemetry_menu`, `pitch_eula`. The game asks for the `glamdoc` menu, it's not in the registry, nothing spawns. This also affects the "unofficial 2.50 build" of Mod Settings, because that rebuild only touched the DLL and kept the old archive.

**Fix:** there is a pull request in the Mod Settings repo with the archive rebuilt from the 2.5 originals (same `mod_settings_main` entry, nothing else changed): https://github.com/jackhumbert/mod_settings/pull/34 (by butrico31). Replace `red4ext\plugins\mod_settings\ModSettings.archive` with the one from that branch. Verified on 2.5 (3.0.88.46235): Glamdoc lists its styles again, Cyberware-EX extra slots work at regular ripperdocs as before.

If you don't want to patch, removing `ModSettings.archive` also fixes the Glamdoc, but you lose the Mod Settings entry in the pause menu.

How to tell if you have this problem: the Cyberware tab in the hub menu works fine, regular ripperdocs work fine, only the Glamdoc is blank, and the blank screen shows no tabs and no level/street cred on the left, just the money on the right.
