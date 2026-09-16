# STRATA Shell 1.0.5

Stable promotion of the Preview build validated on the primary desktop and ASUS dual-display laptop. This release combines the new keybinding workflow, adaptive display and multi-monitor foundations, first-party app refinements, and a safer update/install path.

## Highlights

### Keybindings and command workflow

- Opens the streamlined keybinding viewer with `Super + K` and the matching editor with the protected `Super + Ctrl + K` shortcut.
- Supports conflict checking, per-binding **Default**, and custom add, edit, delete, and reset actions.
- Uses a searchable STRATA Command-style catalog for commands and installed applications.
- Accepts `SUPER` or `WIN` in edit mode and renders the Windows-key icon in the viewer and editor.
- Keeps hardware volume controls active without listing the self-explanatory volume rows, and maps `Super + Ctrl + T` to Task Manager.

### Displays, workspaces, and windows

- Adds recommended adaptive UI baselines for 720p, 1080p, 1440p, 3K, and 4K+ workspaces, with a user adjustment and one-click recommended reset.
- Establishes per-display top rails, workspaces, active-app context, desktop-view state, and reserved work areas while keeping secondary-monitor widgets disabled by default.
- Compensates for invisible DWM resize frames, restores active-window borders, and applies theme-aware native borders.
- Improves repeated directional focus and window routing across workspaces and monitors while modifiers remain held.
- Refines floating-window movement, expansion, and tiled geometry reconciliation.

### First-party apps and shell polish

- Adds sortable STRATA Files columns, groups hidden-file and thumbnail controls, and improves compact ScreenPad layouts.
- Starts STRATA Image Viewer in aspect-preserving fit mode while retaining zoom and actual-size controls.
- Reworks the power panel into clear boxed actions without redundant battery text.
- Includes fresh-install AI Command defaults, Screensaver placement under UI & Theme, animated About branding, and Floating/Tiled notifications.
- Includes the STRATA first-party app suite: Files, Image Viewer, Video Player, Paint, Terminal, Text, and Snip.

### Updates and recovery

- Prevents an update from deleting or overwriting an immutable running release and selects a collision-safe release folder when needed.
- Uses isolated update attempts, persistent error logs, a foreground failure dialog, and **Copy Error** for diagnostics.
- Makes optional ChatGPT discovery non-blocking and reports failing self-test names.
- Preserves reversible Explorer recovery and verifies installer changes against isolated registry roots.

## Stable package

- Self-contained STRATA Shell Setup 1.0.5 for Windows x64, including the required .NET 9 runtime.
- Full self-contained portable ZIP.
- SHA-256 checksum, release manifest, and validation report.
- Exactly the two approved library wallpapers: `STRATA_LOGO_LIGHT.jpg` and `STRATA_LOGO_DARK.jpg`, with the original internal STRATA fallback retained in the application.

## Current boundary

Device-specific ASUS ScreenPad sizing and multi-monitor polish remain ongoing. The adaptive display and per-monitor workspace foundations in this release are the accepted Stable baseline for that continued work.
