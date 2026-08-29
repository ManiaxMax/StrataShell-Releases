# Keybindings

`Super` means the Windows-logo key. The current policy contains 94 unique bindings: 64 shell/application/window/media/monitor/recovery bindings plus 30 direct workspace bindings.

Press `Super + K` inside Strata for the searchable live list. The live list also reports any registration failures.

## Launch and shell

| Shortcut | Action |
|---|---|
| `Super + Space` | Toggle the command menu |
| `Super + Alt + Space` | Toggle the application-only launcher |
| `Super + A` | Open Antigravity |
| `Super + Enter` | Open Strata Terminal |
| `Super + Ctrl + Enter` | Open PowerShell |
| `Super + Ctrl + S` | Open Strata Snip |
| `Super + B` | Open a new browser window |
| `Super + F` | Open Strata Files |
| `Super + S` | Open Strata Settings |
| `Super + Shift + S` | Start the Strata screensaver immediately |
| `Super + -` | Remove the highest workspace when it has no open window; stop at 1 |
| `Super + +` | Add one workspace; stop at 10 |
| `Super + C` | Activate ChatGPT / Codex |
| `Super + K` | Open the searchable keybinding list |
| `Super + Escape` | Open the power/session panel |

The launcher and keybind window place keyboard focus in their search field on every activation. Typing can begin immediately; Down selects the first result and Enter activates it.

## Clipboard

| Shortcut | Action |
|---|---|
| `Super + V` | Universal paste |
| `Super + X` | Universal cut |

These forward the ordinary clipboard action to the currently focused application.

## Center Stage windows

| Shortcut | Action |
|---|---|
| `Super + Q` | Close active window |
| `Super + T` | Toggle transparency for the active third-party or Strata window |
| `Super + W` | Toggle every window on the active workspace between tiled and floating |
| `Super + D` | Cycle Center Stage → wide side-by-side → wide top/bottom |
| `Super + \` | Toggle the active app between glass-expanded and Center Stage |
| `Super + Shift + \` | Toggle true opaque fullscreen and Center Stage |
| `Super + Arrow` | Focus the next existing window in that spatial direction |
| `Super + Shift + Arrow` | Move the active window one spatial position, filling or swapping as needed |

If the destination slot is occupied, Strata exchanges the displaced application with the source slot. Wide desktop views hide widgets while windows smoothly expand into their space. Floating mode keeps every app on the active workspace movable and transparent outside the tiler, remembers each floating size and position, and gives the active float precedence over only the app windows or widget cards it overlaps. Hold `Ctrl` and left-drag anywhere inside a floating window to move it; tiled windows remain locked. Opening or moving another app into that workspace restores its tiled layout. Both expanded modes hide the other app on that workspace until Center Stage returns. Glass-expanded mode retains transparency and the top rail; true fullscreen suspends the active app's transparency and hides the rail. The rail also ducks if any managed window physically crosses it and returns when clear.

### Directional routing rules

| Monitor layout | Left / Right | Up / Down |
|---|---|---|
| Top/bottom | Traverse workspaces; focus skips empty workspaces, while move fills them one at a time | Traverse top/bottom slots, then continue to the adjacent monitor at the edge |
| Side-by-side | Traverse both slots and workspaces as one bounded horizontal chain | Traverse monitors |

Directional focus never wraps and never switches to an empty workspace. Directional move never skips an empty intermediate workspace. Crossing monitors remembers each monitor's active workspace and most recently focused app; movement uses the next available slot on the destination monitor. With one monitor, an arrow that would leave the display simply does nothing.

## Workspace navigation

| Shortcut | Action |
|---|---|
| `Super + Tab` | Next workspace |
| `Super + Shift + Tab` | Previous workspace |
| `Super + Ctrl + Tab` | Former workspace |
| `Super + 1` … `Super + 9` | Switch to workspace 1 … 9 |
| `Super + 0` | Switch to workspace 10 |
| `Super + Shift + 1` … `9` | Move/swap active app to workspace 1 … 9 and follow |
| `Super + Shift + 0` | Move/swap active app to workspace 10 and follow |
| `Super + Shift + Alt + 1` … `9` | Move/swap active app to workspace 1 … 9 without following |
| `Super + Shift + Alt + 0` | Move/swap active app to workspace 10 without following |

Direct moves preserve the current top/bottom layer when possible.

Workspace-count shortcuts update the persisted Workspace Count setting and rail immediately. Removal is blocked when the highest numbered workspace still contains a window on any monitor.

## Monitor routing

| Shortcut | Action |
|---|---|
| `Super + Ctrl + 1` … `4` | Select physical monitor 1 … 4 |
| `Super + Ctrl + Shift + 1` … `4` | Move the active app to physical monitor 1 … 4 and follow |

Directional Up/Down also crosses monitors when the active view reaches its vertical edge. Direct monitor chords remain available regardless of whether that monitor currently uses Center Stage, side-by-side, or wide top/bottom view.

## Appearance and wallpaper

| Shortcut | Action |
|---|---|
| `Super + Shift + Backspace` | Toggle Center Stage gaps |
| `Super + Shift + Space` | Show/hide the top rail |
| `Super + Ctrl + Space` | Open the wallpaper chooser |
| `Super + Ctrl + Shift + Space` | Cycle Auto/Dark/Light appearance |
| `Ctrl + Super + Up` | Activate the Light wallpaper collection |
| `Ctrl + Super + Down` | Activate the Dark wallpaper collection |
| `Ctrl + Super + Left` | Previous wallpaper in the active collection |
| `Ctrl + Super + Right` | Next wallpaper in the active collection |

## Panels and system

| Shortcut | Action |
|---|---|
| `Super + Ctrl + A` | Audio panel |
| `Super + Ctrl + B` | Bluetooth panel |
| `Super + Ctrl + W` | Network panel |
| `Super + Ctrl + D` | Display panel |
| `Super + Ctrl + Alt + D` | Clock/calendar panel |
| `Super + Ctrl + P` | Power panel |
| `Super + Ctrl + H` | Hardware/system panel |
| `Super + Ctrl + Q` | Calculator |
| `Super + Ctrl + T` | Task Manager/activity |
| `Super + Ctrl + L` | Lock the system |

## Media keys

| Key | Action |
|---|---|
| Volume Up | Raise output volume by 5% and show the OSD |
| Volume Down | Lower output volume by 5% and show the OSD |
| Volume Mute | Toggle output mute and show the OSD |

Additional play/pause, previous/next, output-device, and microphone controls are available through the audio panel and spectrum widget.

## Recovery chords

| Shortcut | Preview mode | Installed shell mode |
|---|---|---|
| `Ctrl + Alt + Shift + Delete` | Exit Strata preview | Request permanent Explorer restoration and exit |

These chords intentionally do not contain `Super`, so they remain available if the Windows-key state or the ordinary shell hotkey path becomes unreliable.

## Retired bindings

The code rejects these stale bindings so the former tiling model cannot silently return:

```text
Super + Alt + Arrow
Super + J
Super + P
Super + Backspace
Super + Ctrl + Shift + F
Super + Shift + Enter
Super + Shift + B
Super + Shift + F
Super + Ctrl + O
```

The current assignments are authoritative: `Super + T` is transparency, `Super + W` is tiled/floating, and `Super + \` is glass-expanded/Center Stage.

## Registration notes

Strata uses a low-level keyboard path plus explicit binding validation. A chord can still be unavailable if another elevated application, driver utility, accessibility tool, or global-hotkey program consumes it first. Open `Super + K` to inspect registration failures and compare privilege levels before diagnosing the layout manager.
