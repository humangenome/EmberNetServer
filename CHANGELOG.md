# Changelog

All notable changes to EmberNet are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and EmberNet uses
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Each version is split into **Server** (EmberNetServer and the mod it runs) and
**Client** (the EmberNet app and the mod it installs). A release publishes the
section for its own version verbatim as the release body, so a version with no
section here cannot be released.

## [Unreleased]

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
