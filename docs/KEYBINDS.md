# STRATA Shell keybindings

STRATA application title bars can also be dragged without modifiers. In Tiled mode this releases that app into its per-window floating presentation; `Super + Ctrl + right-click` returns it to its tile. Clicking a running dock icon in the full Floating environment always activates and restores it.

`Super` means the Windows-logo key. The default catalog contains 74 unique bindings, with **72 active in Tiled mode** and **64 active in Floating mode**. The live list and editor filter mode-specific actions; saved remaps remain available when their mode returns.

| Mode shortcut | Action |
|---|---|
| `Super + Shift + W` | Switch the entire shell between Tiled and Floating |
| `Alt + Tab` / `Alt + Shift + Tab` | Floating: select next/previous open app; release Alt to activate, Escape to cancel |
| `Super + Shift + D` | Floating: Show Widgets / Hide Widgets; Tiled: existing three desktop views |

Floating mode disables spatial focus/movement arrows, expansion cycling, and gap toggling. Workspace switching, `Super + Shift + number` moves, monitor routes, app launching, transparency, close, and recovery remain active. Title bars and window edges work without modifiers. Bottom-bar panels and dock context menus open upward. The Center Stage instructions below describe Tiled mode.

Press `Super + K` inside STRATA Shell for the searchable live list. The list renders a code-native Windows-logo icon in place of the word `Super`, without depending on an installed icon font, groups related focus/movement/layout/workspace/monitor actions together, places custom shortcuts in a final **Custom Keybinds** section, and reports any registration failures. The three self-explanatory physical volume keys remain active but are intentionally omitted from the list and editor.

Press `Super + Ctrl + K` for the editor. Built-in actions can be remapped and individually reset with their own **Default** button. User-created shortcuts can be added, edited, or deleted. The action picker uses the searchable STRATA Command catalog, including shell commands and discovered applications. Type either `SUPER` or `WIN` to specify the Windows-logo key while editing. STRATA Shell rejects a requested chord when another active binding already owns it, and the editor plus recovery chords are permanently protected so the editor cannot lock itself out.

## Launch and shell

| Shortcut | Action |
|---|---|
| `Super + Space` | Toggle the command menu |
| `Super + Alt + Space` | Toggle the application-only launcher |
| `Super + Enter` | Open STRATA Terminal |
| `Super + S` | Open STRATA Snip |
| `Super + B` | Open a new browser window |
| `Super + F` | Open STRATA Files |
| `Super + Ctrl + S` | Open STRATA Settings |
| `Super + -` | Remove the highest workspace when it has no open window; stop at 1 |
| `Super + +` | Add one workspace; stop at 10 |
| `Super + K` | Open the Keybinding list |
| `Super + Ctrl + K` | Open the Keybinding Editor (unmodifiable permanent shortcut) |

The dock STRATA icon opens Applications. Power/session options remain available from the always-visible power button; Super + Escape is no longer a built-in shortcut.

The launcher and both keybind modes place keyboard focus in their search field on every activation. Typing can begin immediately.

## Center Stage windows

| Shortcut | Action |
|---|---|
| `Super + Q` | Close active window |
| `Super + Shift + T` | Toggle transparency for the active third-party or STRATA window |
| `Super + Shift + D` | Cycle Center Stage → wide side-by-side → wide top/bottom |
| `Super + Shift + \` | Cycle window expansion: glass-expanded → true fullscreen → Center Stage |
| `Super + Arrow` | Focus the next existing window in that spatial direction |
| `Super + Shift + Arrow` | Move the active window one spatial position, filling or swapping as needed |

If the destination slot is occupied, STRATA Shell exchanges the displaced application with the source slot. Wide desktop views hide widgets while windows smoothly expand into their space. Floating mode allows individual windows to be freely moved and resized outside the tiler while keeping tiled windows organized in Center Stage. Hold `Super + Ctrl` and left-click/left-drag anywhere inside a window to float and move it, or drag along its borders/corners to resize. Hold `Super + Ctrl` and right-click a floating window to restore it to tiled Center Stage mode. A floating window gives visual precedence over only the app windows or widget cards it physically overlaps. `Super + Shift + \` smoothly cycles between glass-expanded mode (which retains theme transparency and the top rail), true opaque fullscreen (which suspends transparency and hides the rail), and regular Center Stage. Both expanded modes hide the sibling app on that workspace until Center Stage returns. The rail also ducks if any managed window physically crosses it and returns when clear.

### Directional routing rules

| Monitor layout | Left / Right | Up / Down |
|---|---|---|
| Top/bottom | Traverse workspaces; focus skips empty workspaces, while move fills them one at a time | Traverse top/bottom slots, then continue to the adjacent monitor at the edge |
| Side-by-side | Traverse both slots and workspaces as one bounded horizontal chain | Traverse monitors |

Directional focus never wraps and never switches to an empty workspace. Directional move never skips an empty intermediate workspace. Crossing monitors uses the destination monitor's currently active workspace and most recently focused app in that workspace; movement fills or swaps within that same visible workspace. With one monitor, an arrow that would leave the display simply does nothing. Arrow bindings repeat while their modifiers remain physically held, including across repeated monitor/workspace moves; releasing and pressing `Super + Shift` again is not required.

## Workspace navigation

| Shortcut | Action |
|---|---|
| `Super + Tab` | Next workspace |
| `Super + Shift + Tab` | Previous workspace |
| `Super + Ctrl + Tab` | Former workspace |
| `Super + Ctrl + Left` | Previous workspace on the active monitor |
| `Super + Ctrl + Right` | Next workspace on the active monitor |
| `Super + 1` … `Super + 9` | Switch to workspace 1 … 9 |
| `Super + 0` | Switch to workspace 10 |
| `Super + Shift + 1` … `9` | Move/swap active app to workspace 1 … 9 and follow |
| `Super + Shift + 0` | Move/swap active app to workspace 10 and follow |
Direct moves preserve the current top/bottom layer when possible.

Workspace hotkeys operate on the monitor containing the foreground window. Each monitor retains its own current and previous workspace, so switching one display never changes another display's visible workspace. Workspace-count shortcuts update the persisted Workspace Count setting and every rail immediately. Removal is blocked when the highest numbered workspace still contains a window on any monitor.

### Touchpad & touchscreen gestures

STRATA Shell features built-in 3-finger precision touchpad and touchscreen swipe gestures:
- **3-finger swipe Left**: Next workspace on the active monitor
- **3-finger swipe Right**: Previous workspace on the active monitor
- **3-finger swipe Down**: Next active monitor ($1 \rightarrow 2 \rightarrow 3 \rightarrow \dots$)
- **3-finger swipe Up**: Previous active monitor ($3 \rightarrow 2 \rightarrow 1 \rightarrow \dots$)

## Monitor routing

| Shortcut | Action |
|---|---|
| `Super + Ctrl + Down` | Next active monitor (cycle forward: 1 → 2 → 3 …) |
| `Super + Ctrl + Up` | Previous active monitor (cycle backward: 3 → 2 → 1 …) |
| `Super + Alt + 1` … `4` | Select physical monitor 1 … 4 and focus its first app in its current workspace |
| `Super + Alt + Shift + 1` … `4` | Move the active app to physical monitor 1 … 4 in that monitor's current workspace and follow |

Every monitor has its own top rail, workspace selection, and active-app context. The rail that owns keyboard workspace routing shows **Active Monitor** after its workspace numbers. Directional Up/Down also crosses monitors when the active view reaches its vertical edge. Direct monitor chords remain available regardless of whether that monitor currently uses Center Stage, side-by-side, or wide top/bottom view.

## Appearance and wallpaper

| Shortcut | Action |
|---|---|
| `Super + Shift + Backspace` | Toggle Center Stage gaps |
| `Super + Shift + Space` | Show/hide the top rail |
| `Super + Ctrl + Space` | Open the wallpaper chooser |
| `Super + Alt + Up` | Activate the Light wallpaper collection |
| `Super + Alt + Down` | Activate the Dark wallpaper collection |
| `Super + Alt + Left` | Previous wallpaper in the active collection |
| `Super + Alt + Right` | Next wallpaper in the active collection |

## Panels and system

| Shortcut | Action |
|---|---|
| `Super + Ctrl + T` | Open Task Manager |
| `Super + Ctrl + L` | Lock the system |

## Media keys

| Key | Action |
|---|---|
| Volume Up | Raise output volume by 5% and show the OSD |
| Volume Down | Lower output volume by 5% and show the OSD |
| Volume Mute | Toggle output mute and show the OSD |

Additional play/pause, previous/next, output-device, and microphone controls are available through the audio panel and spectrum widget.

These physical volume controls remain registered at runtime but are intentionally omitted from the searchable keybind list and editor.

## Recovery chords

| Shortcut | Preview mode | Installed shell mode |
|---|---|---|
| `Ctrl + Alt + Shift + Delete` | Exit STRATA Preview | Request permanent Explorer restoration and exit |

These chords intentionally do not contain `Super`, so they remain available if the Windows-key state or the ordinary shell hotkey path becomes unreliable.

## Retired bindings

The code rejects these stale bindings so the former tiling model cannot silently return:

```text
Super + J
Super + P
Super + Backspace
Super + Ctrl + Shift + F
Super + Shift + Enter
Super + Shift + B
Super + Shift + F
Super + Ctrl + O
Super + W
Super + D
Super + T
Super + \
Super + A
Super + C
Super + Shift + S
Super + Ctrl + Return
Super + V
Super + X
Super + Ctrl + Q
Super + Ctrl + A
Super + Ctrl + Alt + D
Super + Ctrl + B
Super + Ctrl + D
Super + Ctrl + H
Super + Ctrl + P
Super + Ctrl + W
Super + Ctrl + Shift + Space
Super + Ctrl + 1…4
Super + Ctrl + Shift + 1…4
Super + Shift + Alt + 0…9
```

The current assignments are authoritative: `Super + Shift + T` controls transparency, `Super + Shift + D` cycles the active mode's desktop views, and `Super + Shift + W` switches the entire environment. In Tiled mode, `Super + Shift + \` cycles expansion and the held `Super + Ctrl` pointer gesture still floats an individual window.

## Registration notes

STRATA Shell uses a low-level keyboard path plus explicit binding validation. A chord can still be unavailable if another elevated application, driver utility, accessibility tool, or global-hotkey program consumes it first. Open `Super + K` to inspect registration failures and compare privilege levels before diagnosing the layout manager.
