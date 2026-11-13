# Changelog

All notable changes to EmberNet are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and EmberNet uses
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Each version is split into **Server** (EmberNetServer and the mod it runs) and
**Client** (the EmberNet app and the mod it installs). A release publishes the
section for its own version verbatim as the release body, so a version with no
section here cannot be released.

## [Unreleased]

## [0.2.4] - 2026-11-13

### Server

#### Added

- The server download now includes EmberNet's licence and the notices for the third-party code it carries (BepInEx and the libraries it loads). The source code of UnityDoorstop 4.5.0, which the server download ships unchanged, is attached to this release.

#### Removed

- Test-only start switches no longer work in a normal game. They were for our automated tests, for example a player who takes no damage, and any player could turn them on by adding them to the game's start line.
- If you run your own EmberNet server, update it to 0.2.4: on older versions any player on it could turn on these test cheats.

### Client

#### Added

- The app and the installer now include EmberNet's licence and the notices for the third-party code they carry. The source code of UnityDoorstop 4.5.0, which the installer places in your game folder unchanged, is attached to this release.

#### Changed

- The app carries the November 2026 .NET 10 security update.

#### Fixed

- EmberNet starts on Windows builds that only partly support a newer processor safety feature. On those PCs 0.2.3 could stop with "Your Windows doesn't fully support CET". If that happened to you, download `EmberNetSetup-latest.exe` from the EmberNet release page and run it once; your saved servers and settings are kept.
- EmberNet uses about half the processor time while it is minimized or in the tray, and Exit from the tray no longer hangs.

## [0.2.3] - 2026-10-30

### Client

#### Changed

- The EmberNet app runs on .NET 10, replacing .NET 8, which Microsoft stops supporting on November 10, 2026.

#### Fixed

- Updating the app no longer leaves it closed when your Windows user folder has an accented letter, `!` or `%` in its name. If that happened to you, download `EmberNetSetup-latest.exe` from the EmberNet release page and run it once; your saved servers and settings are kept.
- The app no longer restarts to update while you are joining a server. It waits until you are in, as it already did while Emberville was running.
- After an update, the app comes back minimized if it was minimized or in the tray, instead of opening a window over your game.

## [0.2.2] - 2026-10-28

### Client

#### Fixed

- Typing in chat no longer sets off game keys: letters no longer open the inventory, map, quests or other menus or use what your character is standing next to, Space no longer makes your character dodge, and Esc closes the chat line without opening the pause menu.

## [0.2.1] - 2026-10-22

The first EmberNet release: online co-op for the free Emberville demo, played on a dedicated server. Every player in the party installs EmberNet; stock Emberville cannot connect to an EmberNet server.

### Server

#### Added

- Runs the Emberville demo as a dedicated server on Windows, with no screen and no graphics card. Players join it by IP and port.
- The server runs the monsters, bosses, chests, doors, levers, quests and story for the whole party. A monster one player hits is hurt for everyone, the party moves between maps together, and story scenes wait for every player.
- A join password and a player limit, 4 by default and up to 16, set in `EmberNetServer\server.json` with the server's name, port and world.
- The world is saved every few minutes and on shutdown, with an hourly backup kept six deep.
- A status file the server writes and a commands file it reads (`save`, `say`, `kick`, `shutdown`), so a script or a control panel can see who is on and run it.
- The server answers a status question on its game port (name, players, version, password), which the EmberNet app shows in its server list.

### Client

#### Added

- The EmberNet app: add a server's `ip:port`, see whether it is up, who is on it and which version it runs, pick a character and click Join. The app installs the mod and starts Emberville with that character.
- A MULTIPLAYER menu in Emberville itself, to join a server by address.
- Bring your single-player character to a server, or make a new one there. The app lists, renames, deletes and restores the characters on your PC.
- Every player sees the others with their own look and name, and Enter opens the party's text chat.
- Remove from Emberville in the app's Settings takes the mod out again. Single-player is unchanged with it in.
