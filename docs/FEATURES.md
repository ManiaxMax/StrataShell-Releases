# Implemented features and limitations

This document describes the current StrataShell build. It is an implementation inventory, not a promise that every Windows workflow has already been replaced.

## Center Stage window management

- Ten logical workspaces are available by default.
- A workspace contains either one full-height centered application or two applications split into equal top and bottom slots.
- Both slots use the same visible width, including compensation for invisible DWM resize borders.
- The center lane is vertically aligned to the visible top and bottom edges of the widget cards.
- Opening a third application assigns it to the next workspace with capacity and follows it there.
- Moving an application into an occupied slot exchanges the displaced application back to the source workspace.
- `Super + Arrow` follows the next existing app in the active monitor's bounded spatial path, adapting to top/bottom or side-by-side view.
- `Super + Shift + Arrow` moves or swaps the active application with animated transitions.
- `Super + \` toggles glass-expanded mode, retaining transparency/effects and the rail while hiding the workspace sibling and overlapped widgets.
- `Super + Shift + \` toggles true edge-to-edge fullscreen, temporarily disabling that app's transparency and auto-hiding the rail until the pointer reaches the top edge.
- `Super + T` independently toggles whole-window alpha for a third-party app and native material transparency for active Strata windows such as Settings, Files, the wallpaper chooser, and the keybind viewer.
- `Super + W` toggles workspace floating mode. The active floating window hides only the app windows and widget cards its rectangle covers; moving it away restores them. Hold `Ctrl` and left-drag anywhere inside any managed floating window, including borderless third-party windows, to move it.
- The top rail fades and becomes pointer-transparent whenever a managed window crosses its bounds. Touching the top screen edge temporarily reveals the rail, which ducks again after the pointer leaves while the overlap remains.
- A theme-colored border identifies the active application.

Strata tracks top-level application windows through Win32/DWM. Dialogs, owned tool windows, protected video surfaces, elevated windows, and applications that aggressively restore their own geometry may require compatibility exceptions.

## Workspaces

- Workspaces 1 through 10 have direct shortcuts.
- Directional switching uses a full-distance slide transition.
- Applications can be moved while following them or moved silently.
- `Super + Tab` advances, `Super + Shift + Tab` goes back, and `Super + Ctrl + Tab` recalls the formerly active workspace.
- Widgets and the desktop perimeter remain persistent while application workspaces change.
- `Super + -` removes one empty highest workspace down to a minimum of 1; it refuses to remove a workspace that still contains an open app on any monitor. `Super + +` adds one up to 10. The rail and persisted Settings value update together.

These are logical Strata workspaces, not Windows virtual desktops.

## Shell surfaces

- Wallpaper desktop without desktop icons.
- Appbar-reserving top rail, so centered windows begin below it.
- Four-sided theme-colored surround with adjustable intensity and thickness.
- Rofi-style searchable application launcher and command launcher.
- Searchable keybinding viewer.
- Quick panels and on-screen displays for shell and hardware state.
- First-party Settings, Files, Terminal, Text, and Snip windows using the same palette, opacity, blur, bloom, shadow, and active-window systems.
- A capture button in the top rail enters rectangle selection immediately; `Super + Ctrl + S` opens the full Strata Snip utility.

## Strata screensaver

- Starts automatically after five minutes without keyboard or mouse input and covers each connected display with an opaque topmost surface.
- Cycles fifteen approved logo effects: Signal Bloom, Horizontal Drift, Chromatic Split, Scan Materialize, Orbital Lock, Digital Rain, Prism Wipe, Shard Reassembly, Pulse Matrix, Ghost Trails, Logo Supernova, Thunder Seal, Firework Assembly, Gravity Rebirth, and Laser Etch.
- Renders the full-resolution Strata mark with dense particles, energy flares, bloom, and crisp theme-reactive radiance. Light themes use a genuinely light field, dark themes use a dark field, High Contrast sharpens the mark, and Reduced Motion removes the particle/wave paths for a calmer cycle.
- Defers automatic activation during presentation mode and full-screen Direct3D states without letting Windows' generic busy state block the idle timer forever. `Super + Shift + S` always starts a manual preview.
- Mouse movement, mouse buttons, wheel input, touch/stylus input, or any keyboard key closes the surface. The triggering launch chord is swallowed without leaving Super or Shift logically held, so keybinds resume immediately.
- Preserves Windows power settings that are already Never or longer, while raising shorter display, sleep, screen-saver, and inactivity-lock timeouts to a 15-minute floor.

## Wallpaper and theme engine

- Dark, light, and automatic appearance modes.
- Separate `WallpapersDark` and `WallpapersLight` libraries.
- Instant previous/next and collection switching with animated transitions.
- Palette extraction from the active wallpaper.
- Automatic light/dark selection when Auto is enabled.
- Theme propagation to shell surfaces, focus borders, charts, widgets, Command Prompt, PowerShell, Windows application-theme preference, and compatible apps' native DWM caption/background/text colors.
- Adjustable vibrancy, glass opacity, blur strength, CRT-style bloom, animation speed, high contrast, and reduced motion.
- Searchable, paginated wallpaper chooser with thumbnails and a large preview.
- Wallpaper selection and themed confirmation dialogs center in the relevant monitor work area instead of inheriting top-rail or side-panel geometry; confirmations remain modal and above application windows.

The repository and official installer bundle 40 curated 4K dynamic wallpapers (20 Light, 20 Dark) under `wallpapers/`, with `assets/wallpapers/strata-signal.png` as fallback.

## Desktop widgets

All first-party widgets inherit the live wallpaper palette, opacity, vibrancy, blur, bloom, and unified UI scale. Every widget has a top-right Fluent gear for its own behavior, while the main Settings surface keeps only shared visibility and appearance controls.

- Weather with location search, Fahrenheit/Celsius choice, conditions, icons, and expanded forecast details.
- Local time with calendar expansion.
- Focus timer with configurable duration, completion notification, and reminder text.
- Performance telemetry with CPU, memory, network, process counts, expanded process management, and close/end-task actions.
- WASAPI spectrum sourced from the active Windows output mix or default microphone, with band/mirror/level rendering, sensitivity control, and a compact previous/pause/play/next strip that directly controls the embedded YouTube player and falls back to system media transport.
- AI CLI panel with one-click provider selection for Antigravity (`agy`), Codex, and Claude, streaming conversation output, stop/reset actions, and working-directory selection.
- YouTube search/player hosted locally in WebView2, centered playback, optional JPEG previews, autoplay/speed controls, browser account/home routes, and uninterrupted audio/video while the card yields space to another expanded widget.

The AI CLI executables are not bundled. YouTube account authentication stays in the default browser rather than the embedded player, and playback depends on YouTube's embeddable-player availability.

## Application discovery and launch

- Desktop and Start Menu shortcuts.
- Registered executable applications.
- Packaged/MSIX applications.
- Direct launchers for Strata Terminal, Strata Text, Strata Snip, external PowerShell, the default browser, Strata Files, Settings, and ChatGPT.
- Browser launch requests force a new browser window rather than opening only a new tab.
- Search results receive keyboard focus immediately; the first Down key selects the first result, Enter launches it, and a single click launches a result.
- New application windows are focused and assigned to Center Stage as soon as their top-level window becomes manageable.

`Super + Enter` opens Strata Terminal with a persistent CMD session. `Super + Ctrl + Enter` deliberately remains the external PowerShell route. See [First-party applications](FIRST_PARTY_APPS.md) for the editor, capture, terminal, and file-selection contracts.

Discovery cannot guarantee launch metadata for portable software that creates no shortcut or registration entry.

## Strata Files

- Explorer-independent navigation and rendering.
- Known Folder resolution for redirected, OneDrive, and network-backed user folders.
- Home, Desktop, Downloads, Documents, Pictures, Music, Videos, Wallpapers, and mounted volumes.
- Clearly labeled editable path navigation, Back, Forward, Up, Refresh, and history.
- Debounced search/filter field and optional hidden-file display.
- Persistent Details and virtualized Icon views; large folders are enumerated off the UI thread and committed in one batch.
- Typing while the file surface is focused selects the first matching filename; repeated letters cycle matches without taking focus from navigation.
- Icon thumbnails prioritize visible, selected, and newly changed files and appear individually as soon as each provider completes.
- Common image previews use a fast direct decoder; videos, PDFs, and codec-backed formats use the registered Windows thumbnail provider.
- Strata-owned open, save, folder, import, export, wallpaper, script, terminal-output, and working-directory actions use the full themed Strata Files selection mode rather than Windows Explorer's file dialog.
- Multi-select, keyboard navigation, single/double activation behavior, and drag/drop.
- New folder, rename, cut, copy, paste, copy path/full path, properties, and move to Recycle Bin.
- Native Strata Recycle Bin view across local drives with original names/locations, deletion timestamps, Restore, permanent Delete, and Empty Recycle Bin actions.
- File-system watcher updates and visible error/status reporting.
- The active location is highlighted in the Places rail.
- Dark/light context menus and an icon toolbar.

Strata Files is intentionally still a focused file manager. Windows shell extensions, library authoring, advanced ACL editing, archive mounting, search indexing, and every Explorer namespace are not yet reproduced.

## Hardware and session controls

- Master volume, mute, output-device selection, microphone mute, and media transport.
- Display brightness when the hardware exposes a supported WMI brightness interface.
- Wi-Fi, wired-LAN, and offline link detection with distinct top-rail states.
- In-shell Wi-Fi scanning, saved/open/secured network connection, password entry, and disconnect controls in both the rail panel and Settings.
- In-shell Bluetooth classic/LE discovery, pairing, connection, forced disconnect, and unpair controls in both the rail panel and Settings.
- Output-device selection is available in the top-bar audio panel, Settings, and the expanded spectrum widget.
- Lock, sleep, hibernate, sign out, restart, and shutdown; destructive actions require confirmation.
- A confirmed Explorer Session closes Strata and launches the normal Windows desktop only for the current login while preserving Strata as the next sign-in shell. Permanent Explorer restoration remains isolated under Settings → Windows Tweaks and the emergency recovery route.
- Strata owns the ordinary network, Bluetooth, and audio workflows. Device-required Bluetooth PIN/consent, UAC, drivers, and other protected handshakes remain Windows-owned.

## Windows lock and sign-in wallpaper

- Every successful Strata wallpaper selection is mirrored asynchronously to the current user's Windows lock screen by default.
- The source image is staged under `%LOCALAPPDATA%\StrataShell\LockScreen` with a fresh Windows-compatible name; JPEG/PNG files are copied and other supported Strata image formats are converted to PNG.
- Rapid wallpaper changes are coalesced so an older request cannot overwrite the newest selection.
- Windows can reuse the resulting lock-screen image on the sign-in screen when **Show the lock screen background picture on the sign-in screen** is enabled.
- **Windows Tweaks → Sync Lock + Sign-In Wallpaper** can stop or resume future synchronization. Disabling it does not unexpectedly replace the current Windows lock-screen picture.
- Strata uses current-user personalization APIs and does not force machine-wide lock/logon policy.

## Dialogs and confirmations

- Strata-owned confirmations, failures, self-test reports, task termination, file recycling, shell changes, and power actions use a wallpaper-theme-aware dialog surface.
- Secure desktop, UAC, driver consent, and hardware pairing ceremonies can still display protected Windows UI because ordinary applications cannot replace those trust surfaces.

## Top rail

Each module can be independently enabled in Settings. The rail reflows when modules are hidden.

- Workspaces
- Context label
- Optional CPU signal
- Active-app icon and name
- Application-tray down-chevron first in the right-side status group
- Network
- Bluetooth
- Audio
- Direct ChatGPT / Codex launch
- Command launcher
- Settings
- Power/session
- Optional hover panels

The tray dropdown groups registrations by product/company identity. Each base application appears once, while distinct publisher controls remain available in its native-menu submenu. Windows hardware/Bluetooth carrier registrations are filtered from the application tray. Apps such as Steam that close their window but remain alive in the notification area no longer retain a Center Stage slot. Background processes without a published tray icon can be shown or explicitly exited after confirmation.

## Safety and recovery

- Safe preview mode does not change the configured Windows shell.
- The comprehensive self-test validates core layout policies, directional routing, dynamic workspace-count safety, floating-window occlusion/lifecycle, foreground-modal isolation, monitor-centered transient surfaces, Strata Files operations/type selection/Recycle Bin metadata, shared picker policy, Terminal persistence, Text encoding/atomic save, Snip capture/encoding, launcher priority, tray aggregation, Bluetooth physical-device grouping, Windows-edition activation, native caption theming, media compatibility, screensaver idle/input policy, private-update safety, lock-screen synchronization, required files, and machine-dependent integrations.
- Installer publishes immutable timestamped releases beneath `%LOCALAPPDATA%\Programs\StrataShell\releases`.
- Windows 11 Home/Pro use a reversible current-user custom-shell policy. Supported Enterprise/Education/IoT editions use Windows Shell Launcher, retain `explorer.exe` as its default fallback, and assign Strata only to the current user SID.
- Bootstrap watchdog restores Explorer after three startup failures inside 45 seconds.
- `Super + C` opens ChatGPT / Codex from the shell.
- `Ctrl + Alt + Shift + Delete` exits preview or requests Explorer restoration in installed shell mode.
- External recovery scripts and a Start Menu recovery shortcut remain available outside the shell process.

See [Installation and recovery](INSTALLATION.md) before enabling default-shell mode.

## Current technical boundaries

- Strata is a shell and window orchestrator, not a replacement for the Windows kernel, DWM, security desktop, driver stack, or application frameworks.
- Third-party transparency is whole-window opacity. Strata cannot transform an unrelated application's internal controls into native acrylic.
- True Compiz-style mesh deformation of third-party pixels is not implemented. First-party shell surfaces use lightweight spring motion, scale, opacity, glow, blur, and bloom.
- Native caption-button hiding is best effort. Some applications redraw or depend on their own chrome.
- Microsoft Media Player is constrained to one primary stage surface and kept opaque for hardware-video compatibility; owned playback/control surfaces do not become separate workspace apps.
- Secure Windows surfaces remain Windows-owned by design.
- Multi-monitor routing and per-monitor view-state policy are implemented and covered by deterministic checks; physical acceptance on a multi-monitor machine remains pending.
- The project is alpha software and does not currently ship signed binaries. Automatic updates require an authenticated account that can read the private repository.
