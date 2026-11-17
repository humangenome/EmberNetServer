<p align="center">
  <img src="docs/img/embernet-lockup.png" alt="EmberNet" width="420">
</p>

<p align="center">
  <a href="#requirements"><img src="https://img.shields.io/badge/Platform-Windows_10%2F11%2FServer-blue.svg" alt="Platform"></a>
  <a href="https://store.steampowered.com/app/2295170/"><img src="https://img.shields.io/badge/Game-Emberville-30e1b9.svg" alt="Game"></a>
  <a href="https://github.com/HumanGenome/EmberNet"><img src="https://img.shields.io/badge/Player_App-EmberNet-brightgreen.svg" alt="Player app"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="License"></a>
</p>

# EmberNetServer

[Emberville](https://store.steampowered.com/app/2295170/) has no multiplayer. EmberNetServer runs an Emberville dedicated server on Windows, for up to 16 players. Players join it by IP with the [EmberNet app](https://github.com/HumanGenome/EmberNet).

## What it does

- Runs with no graphics card
- Saves the world every 5 minutes and when it stops, and keeps the last 6 hourly backups
- Runs the monsters, bosses, chests, doors, quests and story for the whole party, and moves the party between maps together
- Players join by IP on one UDP port
- Password protection, and a player limit you set: 4 by default, up to 16
- A status file shows who is on, and a commands file lets you save, kick, send a message or shut down
- Shows up in the EmberNet app's server list with its name, players, version and lock
- Idles at a few percent of one CPU core with nobody on it

## Known limitations

- No anti-cheat. EmberNet doesn't check what players do.

## Requirements

| | |
|---|---|
| OS | Windows 10, Windows 11, or Windows Server |
| Game files | An Emberville installation on the host, from your own Steam copy |
| Port | One UDP port (`47750` by default), open in the firewall and forwarded on your router if the PC is behind one |
| Hardware | No graphics card. A few MB of disk beyond the game. About 3 GB of RAM per server: in our tests a server with its world loaded used about 2.7 GB, and about 11% of one CPU core with a player fighting |

## Setup

1. Put a copy of Emberville's game files on the host from your own Steam copy: install the game through Steam on the host, or copy the game folder from a PC that has it.
2. Download `EmberNetServer-<version>.zip` from the [latest release](https://github.com/HumanGenome/EmberNetServer/releases/latest) and extract it **into** the game folder, so `winhttp.dll`, `doorstop_config.ini` and `BepInEx\` sit next to `Emberville.exe`.
3. Start the server once from the game folder so it writes its settings, then stop it:

   ```
   Emberville.exe -batchmode -nographics --emb-dedicated
   ```

4. Edit `EmberNetServer\server.json` next to the game: the server name, the port, the player limit, a password if you want one, the world's name and difficulty.
5. Start the server again. The world is made on the first start and reopened on every start after.
6. Give players your IP and port as `ip:port`. They add it in the EmberNet app and click Join, or type it in the game's MULTIPLAYER menu.

Running two servers on one machine: give each its own copy of the game folder and its own port. Each folder keeps its own settings, saves and status files in `EmberNetServer\`.

## Settings (`EmberNetServer\server.json`)

| Key | Default | Meaning |
|---|---|---|
| `ServerName` | `Emberville Server` | The name players see |
| `Port` | `47750` | The UDP port players connect to |
| `MaxPlayers` | `4` | Most players at once (1 to 16) |
| `Password` | empty | Join password. Empty means anyone can join. |
| `WorldName` | `Emberville World` | The world's name, used when the world is first made |
| `Seed` | `0` | The world's seed when it's first made. `0` picks one. |
| `Difficulty` | `1` | The game's difficulty, when the world is first made |
| `StartScene` | `TutorialArea` | The map a new world starts in |
| `SaveEveryMinutes` | `5` | How often the world is saved, in minutes (1 to 60) |
| `FramesPerSecond` | `15` | How fast the server runs with players on (5 to 60). It runs slower with nobody on. |
| `WorkerThreads` | `2` | The engine's helper threads (0 to 8) |

## Status and commands

The server writes `EmberNetServer\status.json` every 5 seconds: its name, world, version, map, players and their names, uptime and the time of the last save.

It reads `EmberNetServer\commands.txt` every second, runs each line, and empties the file:

| Command | Does |
|---|---|
| `save` | Saves the world now |
| `say <text>` | Shows the text to every player |
| `kick <name>` | Kicks that player |
| `shutdown` | Saves and stops the server |

## Saves and backups

Everything a server owns is in `EmberNetServer\` next to the game: `server.json`, the world in `Saves\`, and hourly copies in `Saves\backups\`. Back up that folder and you have the server. Players' characters stay on their own PCs.

## Releases

Each release has the server download, `EmberNetServer-<version>.zip`. Players and the server need the same network version, so update the server when a new release is out. What changed in each version is in [CHANGELOG.md](CHANGELOG.md).

---

EmberNet is a free, unofficial fan-made project. It's not made by or affiliated with Cygnus Cross.
