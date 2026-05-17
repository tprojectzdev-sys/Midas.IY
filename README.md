# Midas.IY

Midas.IY is a Roblox client-side Lua command framework built around an Infinite Yield-style command bar. The runtime provides a graphical command interface, key-gated access, persistent settings, aliases, keybinds, waypoints, plugin loading, event binds, and a large utility command catalog.

## Repository Contents

| File | Purpose |
| --- | --- |
| `midas.lua` | Main runtime script. Contains compatibility shims, key validation, GUI construction, command registration, persistence, plugin loading, and update checks. |
| `version` | JSON metadata used by the runtime to check whether the local script version is current. |
| `README.md` | Project documentation. |
| `.gitignore` | Local ignore rules. |

## Current Status

| Item | Value |
| --- | --- |
| Runtime script version | `6.4.2` inside `midas.lua` |
| Published version metadata | `6.4.3` in `version` |
| Primary language | Lua |
| Runtime target | Roblox client Lua environment |

> Development note: update either `currentVersion` in `midas.lua` or the `version` file so both values match.

## Features

- Command bar UI with draggable panels, notifications, tooltips, and settings views.
- Executor compatibility layer for common APIs such as `request`, `writefile`, `readfile`, `isfile`, `makefolder`, `queue_on_teleport`, `setclipboard`, and custom asset loading.
- Key validation gate using HTTP POST validation, saved key reuse, manual key entry, retry limits, maintenance handling, and HWID/user identity binding.
- Persistent local configuration through `MIDAS.iy`.
- Local key storage through `MIDAS.key`.
- Theme customization, prefix configuration, UI scaling, and menu behavior settings.
- Alias management through command and GUI workflows.
- Keybind management with key-down, key-up, and toggle command support.
- Waypoint management for saving, loading, showing, tweening, walking to, and deleting positions.
- Plugin system for `.iy` plugin files stored in the executor workspace.
- Event bind editor with startup/spawn/leave/execute event support.
- Version check against the remote `version` file.
- Optional asset caching under `midas/assets`.

## Requirements

Midas.IY expects a Roblox Lua execution environment with support for the APIs used by the runtime. Function availability depends on the executor.

### Required for core operation

- `game:HttpGet`
- `request`, `http_request`, `syn.request`, `http.request`, or compatible HTTP request function
- Roblox services available through `game:GetService`

### Required for persistence and local assets

- `writefile`
- `readfile`
- `isfile`
- `makefolder`
- `isfolder`

### Optional features

| API | Used For |
| --- | --- |
| `queue_on_teleport` | Re-running MIDAS after teleport |
| `setclipboard` / compatible clipboard API | Copy commands, IDs, links, and paths |
| `getcustomasset` / `getsynasset` | Loading locally cached UI assets |
| `hookfunction`, `hookmetamethod`, `getgc` | Advanced command/runtime features |
| `saveinstance` | Save-place command support |

## Installation

Run the main script from the raw GitHub URL:

```lua
loadstring(game:HttpGet("https://raw.githubusercontent.com/tprojectzdev-sys/Midas.IY/main/midas.lua"))()
```

## Runtime Flow

1. Prevents duplicate initialization unless debug mode is enabled.
2. Waits for the Roblox game client to finish loading.
3. Builds compatibility wrappers for executor-specific APIs.
4. Caches Roblox services through a lazy service resolver.
5. Runs the MIDAS key validation gate.
6. Loads or downloads UI assets into `midas/assets`.
7. Builds the command UI, settings UI, notifications, tooltips, logs, keybind editor, waypoint editor, plugin editor, and event editor.
8. Loads saved configuration from `MIDAS.iy` when file access is available.
9. Registers built-in commands and plugin commands.
10. Loads saved aliases, plugins, event binds, and spawn commands.
11. Checks the remote `version` file for updates and announcements.

## Local Files

| Path | Purpose |
| --- | --- |
| `MIDAS.key` | Stores the validated key locally after successful validation. |
| `MIDAS.iy` | Stores user settings, prefix, aliases, keybinds, waypoints, plugins, theme values, logs settings, and event bind data. |
| `midas/assets/` | Stores cached UI image assets. |
| `*.iy` | Plugin files loaded from the executor workspace. |
| `* Chat Logs (*.txt)` | Chat log exports created by the chat log save workflow. |

## Key Validation

The runtime validates access before loading the main command framework.

Validation behavior:

- Reads an existing key from `MIDAS.key` when available.
- Sends the key, user ID, and client identity to the validation endpoint.
- Saves the key locally after successful validation.
- Deletes invalid or HWID-mismatched keys.
- Stops immediately for maintenance or HWID mismatch conditions.
- Allows up to three manual key attempts.
- Times out the key entry UI after the configured wait period.

## Command System

Commands are registered with the internal `addcmd` function. Each command can define:

- Primary command name
- Aliases
- Callback function
- Optional plugin source

The current script includes commands for:

- MIDAS visibility and menu control
- Server and player information
- Teleport, rejoin, and server navigation helpers
- Character and camera utilities
- Waypoints
- Keybinds
- Aliases
- Plugins
- Event binds
- Logs and notifications
- GUI tools
- Utility commands

Use the in-game command list and tooltip system to inspect available commands and descriptions at runtime.

## Aliases

Aliases map custom command names to registered commands.

Common alias commands:

```text
addalias <command> <alias>
removealias <alias>
clraliases
```

Alias data is saved in `MIDAS.iy` and restored on startup.

## Keybinds

Keybinds can execute commands from keyboard or mouse input. The runtime supports:

- KeyDown triggers
- KeyUp triggers
- Toggle bindings with two command states
- Left-click and right-click bindings for supported actions

Keybind data is saved in `MIDAS.iy`.

## Waypoints

Waypoints store position data for the current place and can be reused through waypoint commands or the waypoint UI.

Common waypoint workflows:

```text
setwaypoint <name>
waypoint <name>
showwaypoints
hidewaypoints
deletewaypoint <name>
clearwaypoints
```

Waypoint data is saved in `MIDAS.iy`.

## Plugins

Plugins are `.iy` files located in the executor workspace. They can be added through the plugin UI or command system.

Common plugin commands:

```text
addplugin <name>
removeplugin <name>
reloadplugin <name>
addallplugins
```

Basic plugin structure:

```lua
return {
    PluginName = "Example Plugin",
    PluginDescription = "Adds an example command.",

    Commands = {
        example = {
            Aliases = {"ex"},
            Description = "Runs the example command.",
            Function = function(args, speaker)
                print("Example command executed")
            end
        }
    }
}
```

When loaded, plugin commands are registered into the same command index as built-in commands.

## Versioning

The runtime checks the remote `version` file:

```json
{
  "Version": "6.4.3",
  "Announcement": ""
}
```

If `currentVersion` in `midas.lua` does not match `Version`, MIDAS displays an outdated-version notification with the raw script URL.

## Development Notes

Recommended cleanup before larger releases:

- Keep `currentVersion` in `midas.lua` synchronized with the `version` file.
- Add a license file.
- Add a changelog.
- Add release tags instead of relying only on the `main` branch.
- Split the monolithic script into modules:
  - `core/compat.lua`
  - `core/services.lua`
  - `core/persistence.lua`
  - `core/keygate.lua`
  - `ui/main.lua`
  - `ui/settings.lua`
  - `commands/*.lua`
  - `plugins/loader.lua`
- Document the plugin API separately.
- Document command categories separately if the command catalog continues to grow.

## Support

The runtime includes a `discord` command that copies or opens the configured support invite when clipboard and local Discord RPC support are available.

## License

No license file is currently included in the repository. Add a license before distributing, modifying, or accepting external contributions.
