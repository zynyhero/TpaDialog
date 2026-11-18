# TpaDialog

> Teleport requests for ZpizzaSMP, built on native in-game dialog menus.

**TpaDialog** is a teleport request plugin (TPA) for Minecraft servers. Instead of chest GUIs, it uses the game's native **Dialog screens**, so players pick who to teleport to from a clean menu with player faces, pagination and a search bar.

**Authors:** Zynyhero, Claude

## Features

- **Native dialog menus** – no inventory GUIs. `/tpa` and `/tpahere` open a player picker.
- **Player faces** next to each nickname (Minecraft 1.21.9+), without rank prefixes. Buttons grow wider for long nicknames.
- **Confirmation screen** before sending a request, with green *Accept* and red *Cancel* buttons side by side.
- **Clickable chat requests** – the receiver gets a gold message with bold `ACCEPT` / `DENY` buttons they can click.
- **Bedrock support** – Bedrock players (Geyser/Floodgate) can't click chat buttons, so they see `/tpaccept <player>` and `/tpdeny <player>` instead. Detected through Floodgate, or by UUID/prefix as a fallback.
- **Vanish aware** – vanished players are hidden from the menu, tab completion, direct requests, `/tpall` and `/tpaall` (EssentialsX, SuperVanish, PremiumVanish and anything using `hidePlayer`).
- **Real command arguments** – commands use Brigadier, so the client shows the `<jugador>` argument with live suggestions. `/tpaccept` and `/tpdeny` only suggest players who sent you a request.
- **Fully configurable messages** – every text lives in `messages.yml` (MiniMessage format).
- **LuckPerms friendly** – one permission node per command, declared in `plugin.yml` so LuckPerms picks them up automatically.
- **Folia compatible** – uses entity schedulers and `teleportAsync`.

## Requirements

| | |
|---|---|
| Server software | **Paper**, **Purpur** or **Folia** (or another Paper fork) |
| Minecraft | 1.21.8 – 26.3 |
| Java | 21 or newer (25 on 26.x servers) |
| Optional | LuckPerms, Floodgate |

> ⚠️ **Not compatible with Spigot / CraftBukkit.** The plugin relies on Paper's Dialog API and Brigadier command API.
>
> On 1.21.8 the player faces are not shown (text-embedded heads were added in 1.21.9).

## Installation

1. Download `tpadialog.jar` (or build it, see below).
2. Drop it into your server's `plugins/` folder.
3. Restart the server.
4. Edit `plugins/tpadialog/config.yml` and `messages.yml` if you want, then run `/tpadialog reload`.

## Building

```bash
mvn clean package
```

The jar is created at `target/tpadialog.jar`. Requires JDK 21 or newer. The project compiles against the 1.21.8 API so it doesn't use anything missing on the oldest supported version.

## Commands

| Command | Description | Permission |
|---|---|---|
| `/tpa` (alias `/tpask`) | Open the player picker | `tpadialog.tpa` |
| `/tpa <player>` | Send a request to go to a player | `tpadialog.tpa` |
| `/tpahere` | Open the picker; the chosen player comes to you | `tpadialog.tpahere` |
| `/tpahere <player>` | Ask a player to teleport to you | `tpadialog.tpahere` |
| `/tpaccept [player]` | Accept a request (latest one if no name is given) | `tpadialog.tpaccept` |
| `/tpdeny [player]` | Deny a request (latest one if no name is given) | `tpadialog.tpdeny` |
| `/tpcancel` | Cancel all the requests you sent | `tpadialog.tpcancel` |
| `/tpall` | Teleport every player to you | `tpadialog.tpall` |
| `/tpaall` | Send everyone a request to teleport to you | `tpadialog.tpaall` |
| `/tpadialog reload` | Reload `config.yml` and `messages.yml` | `tpadialog.reload` |

## Permissions

| Node | Default | Description |
|---|---|---|
| `tpadialog.use` | everyone | Player commands: `tpa`, `tpahere`, `tpaccept`, `tpdeny`, `tpcancel` |
| `tpadialog.admin` | op | Admin commands: `tpall`, `tpaall`, `reload` and `seevanished` |
| `tpadialog.seevanished` | op | See and target vanished players in the menus |
| `tpadialog.*` | op | Everything |

Each command also has its own node (`tpadialog.tpa`, `tpadialog.tpall`, …) for fine-grained control in LuckPerms.

## Configuration

`config.yml`

```yaml
request-timeout-seconds: 60   # how long a request stays valid
players-per-page: 9           # above this, pagination and search appear
strip-prefix: "."             # prefix removed from nicknames in menus (e.g. Floodgate's ".")
```

`messages.yml` holds every message, button label and dialog title, written in [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Missing keys fall back to the defaults bundled in the jar.

## How it works

1. A player runs `/tpa` and picks someone from the menu (or runs `/tpa <player>` directly).
2. After confirming, the target receives a chat message with **ACCEPT** and **DENY** buttons (or the commands, if they are on Bedrock).
3. If accepted, the teleport happens. Requests expire after `request-timeout-seconds`.

Menu layout: with few players you get a simple one-column list. With more than `players-per-page`, the menu switches to a 3-column grid with the navigation row and search bar at the top.

## Support the project

If you like TpaDialog and want to support its development, you can donate on Ko-fi. It's completely optional and very much appreciated. Thank you! ☕

[![Support me on Ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/zynyhero)

👉 **https://ko-fi.com/zynyhero**

## Authors

Made by **Zynyhero** and **Claude**.
