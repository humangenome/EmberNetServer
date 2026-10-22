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

[Emberville](https://store.steampowered.com/app/2295170/) has no multiplayer and no dedicated server. EmberNetServer runs the game as one on a Windows machine, and players join it by IP and port through the [EmberNet app](https://github.com/HumanGenome/EmberNet) or the MULTIPLAYER menu EmberNet adds to the game. This repo is for hosts; players only need the app.

## What it does

- Runs Emberville with no screen and no graphics card
- Saves the world every few minutes and on shutdown, and keeps an hourly backup six deep
- Runs the monsters, bosses, chests, doors, quests and story for the whole party, and moves the party between maps together
- Takes direct connections on one UDP port
- Optional join password, and a player limit you set (4 by default, up to 16)
- Writes a status file and reads a commands file, so a panel or a script can see who is on and save, kick, broadcast or shut down without a login
- Answers a status question on its game port (name, players, version, password), which the EmberNet app uses for its server list
- Idles at a few percent of one CPU core with nobody on it

## What it does not do

- **It does not ship Emberville.** You install the game files yourself; the server runs them from a folder you choose.
- **It does not replace the player's game.** Everyone joining runs their own Emberville, with EmberNet on top.
- **It is not an anti-cheat.** EmberNet does not police what connected players do.

## Requirements

| | |
|---|---|
| OS | Windows 10, Windows 11, or Windows Server |
| Game files | An Emberville installation on the host, from your own Steam copy |
| Port | One UDP port (default `47750`), open on the host firewall and forwarded if the server sits behind NAT |
| Hardware | No GPU required. A few MB of disk beyond the game. Plan for about 3 GB of RAM per server: in our tests a server with its world loaded used about 2.7 GB, and about 11% of one CPU core with a player fighting |

## Setup

1. Put a copy of Emberville's game files on the host from your own Steam copy: install the game through Steam on the host, or copy the game folder from a PC that has it.
2. Download `EmberNetServer-<version>.zip` from the [latest release](https://github.com/HumanGenome/EmberNetServer/releases/latest) and extract it **into** the game folder, so `winhttp.dll`, `doorstop_config.ini` and `BepInEx\` sit next to `Emberville.exe`.
3. Start the server once from the game folder so it writes its settings, then stop it:

   ```
   Emberville.exe -batchmode -nographics --emb-dedicated
   ```

4. Edit `EmberNetServer\server.json` next to the game: the server name, the port, the player limit, a join password if you want one, the world's name and difficulty.
5. Start the server again. The world is made on the first start and reopened on every start after.
6. Hand players the address as `ip:port`. They add it in the EmberNet app and press Join, or type it in the game's MULTIPLAYER menu.

Running two servers on one machine: give each its own copy of the game folder and its own port. Each folder keeps its own settings, saves and status files in `EmberNetServer\`.

## Settings (`EmberNetServer\server.json`)

| Key | Default | Meaning |
|---|---|---|
| `ServerName` | `Emberville Server` | The name players see |
| `Port` | `47750` | The UDP port players connect to |
| `MaxPlayers` | `4` | Most players at once (1-16) |
| `Password` | empty | Join password; empty means open |
| `WorldName` | `Emberville World` | The world's name, used when the world is first made |
| `Seed` | `0` | The world's seed when it is first made; `0` picks one |
| `Difficulty` | `1` | The game's difficulty, when the world is first made |
| `StartScene` | `TutorialArea` | The map a new world starts in |
| `SaveEveryMinutes` | `5` | How often the world is saved (1-60) |
| `FramesPerSecond` | `15` | How fast the server runs with players on (5-60); it idles slower with nobody on |
| `WorkerThreads` | `2` | The engine's helper threads (0-8) |

## Status and commands

The server writes `EmberNetServer\status.json` every 5 seconds: its name, world, version, map, players and their names, uptime and the time of the last save.

It reads `EmberNetServer\commands.txt` every second, runs each line, and empties the file:

| Command | Does |
|---|---|
| `save` | Saves the world now |
| `say <text>` | Shows the text to every player |
| `kick <name>` | Removes that player |
| `shutdown` | Saves and stops the server |

## Saves and backups

Everything a server owns is in `EmberNetServer\` next to the game: `server.json`, the world in `Saves\`, and hourly copies in `Saves\backups\`. Back up that folder and you have the server. Players' characters stay on their own PCs.

## Releases

Each release attaches `EmberNetServer-<version>.zip`. Players and the server must run the same EmberNet version: the EmberNet app shows a server's version before joining and updates itself to match new releases. The changelog for both sides is [CHANGELOG.md](CHANGELOG.md).

## Is this official?

No. EmberNet is an independent, fan-made project. It is not made by or affiliated with Cygnus Cross.
