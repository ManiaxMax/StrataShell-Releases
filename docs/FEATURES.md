# Implemented features and limitations

This document describes current Preview development. Published Stable releases may predate these changes; see [release and testing status](STATUS.md). It is an implementation inventory, not a promise that every Windows workflow has already been replaced.

## Floating environment

`Super + Shift + W` switches the full environment. Floating mode offers native title-bar movement and edge resize without modifiers, STRATA/native window controls, minimized-app recovery, grouped persistent dock pins, and an Alt + Tab selector. Workspace and monitor moves remain available without a two-window limit. The bottom bar replaces center context text with the launcher and dock; launcher, tray, power, audio and network panels open upward. Show Widgets / Hide Widgets replace the three tiled desktop views until Tiled mode returns. [Behavior, verification and limits](FLOATING_MODE.md).

## Center Stage window management

- One to ten logical workspaces are supported. The release template starts with five; existing users retain their configured count.
- A workspace contains either one full-height centered application or two applications split into equal top and bottom slots.
- Both slots use the same visible width, including compensation for invisible DWM resize borders.
- The center lane is vertically aligned to the visible top and bottom edges of the widget cards.
- Opening a third application assigns it to the next workspace with capacity and follows it there.
- Moving an application into an occupied slot exchanges the displaced application back to the source workspace.
- `Super + Arrow` follows the next existing app in the active monitor's bounded spatial path, adapting to top/bottom or side-by-side view. At a monitor edge it continues through the next monitor's currently visible workspace and updates focus, active-border, rail, and workspace ownership together.
- `Super + Shift + Arrow` moves or swaps the active application with animated transitions. A half-filled destination preserves the incoming app's TOP/BOTTOM slot (or the current left/right spatial boundary), while a full destination exchanges the matching slot. At a monitor edge it fills or swaps within that destination monitor's currently visible workspace. Directional focus and movement continue repeating while the modifiers remain physically held; monitor/workspace moves do not require releasing and pressing `Super + Shift` again.
- `Super + Shift + \` cycles the active application through glass-expanded mode (retaining transparency/effects and the rail while hiding the workspace sibling and overlapped widgets), true edge-to-edge fullscreen (temporarily disabling that app's transparency and auto-hiding the rail), and standard Center Stage.
- `Super + Shift + T` independently toggles whole-window alpha for a third-party app and native material transparency for active STRATA windows such as Settings, Files, the wallpaper chooser, and the keybind viewer.
- Floating mode allows individual windows to be freely moved and resized outside the tiler. Hold `Super + Ctrl` and left-click/left-drag anywhere inside a window to float and move it, or drag along its borders/corners to resize. Pointer samples are collapsed into one latest-position commit per composed frame; STRATA frost and bloom pause only during the gesture and resume after an 85 ms settle, without replacing live content with a stretched snapshot. Hold `Super + Ctrl` and right-click a floating window to restore it to tiled Center Stage layout. The active floating window hides only the app windows and widget cards its rectangle covers; moving it away restores them.
- The top rail fades and becomes pointer-transparent whenever a managed window crosses its bounds. Touching the top screen edge temporarily reveals the rail only for ordinary overlap; native and STRATA true-fullscreen applications keep the active-window border, rail, dock, launcher, and quick panels fully suppressed until fullscreen ends.
- Invisible DWM resize-frame clearance prevents top/bottom and side overlap between Center Stage slots, preserving the theme-colored active-application border.
- Compatible browser and native DWM borders follow the current dark/light wallpaper theme instead of retaining an unrelated light frame.

STRATA Shell tracks top-level application windows through Win32/DWM. Dialogs, owned tool windows, protected video surfaces, elevated windows, and applications that aggressively restore their own geometry may require compatibility exceptions.

## Workspaces

- Workspaces 1 through 10 have direct shortcuts.
- Directional switching uses a full-distance slide transition.
- Applications can be moved while following them; direct workspace and monitor routes retain the destination display's independent state.
- Managed window movement keeps live application content and temporarily suspends expensive visual effects while geometry settles. Dispatcher-driven workspace and slot transitions honor the selected frame cap. Screen/DPI movement and visual fidelity require the hardware acceptance recorded for each candidate.
- `Super + Tab` advances, `Super + Shift + Tab` goes back, and `Super + Ctrl + Tab` recalls the formerly active workspace.
- Every connected monitor owns a dedicated top rail, current/previous workspace state, active-app context, desktop-view mode, and top reservation. The rail currently owning keyboard workspace routes shows **Active Monitor** after its workspace numbers. `Super + Alt + 1…4` selects a monitor and its first visible app; `Super + Alt + Shift + 1…4` moves the active app into that monitor's current workspace. Launchers and quick panels opened from a rail remain on its display. ScreenPad-style secondary displays remain widget-free by default but still show their own rail and workspace position.
- Widgets and the desktop perimeter remain persistent while application workspaces change.
- `Super + -` removes one empty highest workspace down to a minimum of 1; it refuses to remove a workspace that still contains an open app on any monitor. `Super + +` adds one up to 10. The rail and persisted Settings value update together.

These are logical STRATA workspaces, not Windows virtual desktops.

## Shell surfaces

- Wallpaper desktop without desktop icons.
- Dedicated appbar-reserving top rail on every monitor, so centered windows begin below the correct local rail.
- Four-sided theme-colored surround with adjustable intensity and thickness.
- Rofi-style searchable application launcher and command launcher.
- Searchable read-only keybinding viewer on `Super + K` and a protected editor on `Super + Ctrl + K` for remapping/resetting built-ins and adding/editing/deleting conflict-checked command or application shortcuts. Related actions are grouped by function and custom shortcuts occupy a final **Custom Keybinds** section. Each built-in row has its own **Default** action, the action field opens a searchable STRATA Command catalog, and edit mode accepts either `SUPER` or `WIN` for the Windows-logo key.
- Quick panels and on-screen displays for shell and hardware state.
- First-party STRATA Settings, Files, Terminal, Text, Snip, Image Viewer, Media Player, Paint, Task Manager, and Browser windows using the same palette, opacity, independently targetable blur, bloom, shadow, and active-window systems.
- A capture button in the top rail enters rectangle selection immediately; `Super + S` opens the full STRATA Snip utility.

## STRATA screensaver

- Starts automatically after five minutes without keyboard or mouse input and covers each connected display with an opaque topmost surface.
- Cycles 30 distinct logo effects: the original 15 signal, drift, split, scan, orbital, rain, prism, shard, matrix, trail, supernova, thunder, firework, gravity, and laser modes plus Signal Constellation, Vector Fracture, Aurora Weave, Helix Transmission, Quantum Lattice, Plasma Cascade, Spectral Echo, Data Vortex, Neon Monolith, Particle Forge, Temporal Slice, Solar Flare, Hologram Fold, Circuit Bloom, and Event Horizon.
- Renders the shared uppercase About mark as code-native vector geometry with resolution-aware particle density, angular radiance, aurora fields, energy flares, heavy multi-pass phosphor bloom, tiled CRT scanlines, edge vignette, and theme-reactive color. Light themes use a contrast-preserving charcoal-mineral field instead of full white, dark themes stay deep, High Contrast sharpens the mark, and Reduced Motion removes the high-motion particle/wave paths for a calmer cycle.
- Keeps the ambient field continuous while outgoing and incoming logo treatments use a 1.6-second smoother-step transition instead of cutting or drawing two complete scenes at catalog boundaries. All monitors share one time-based animation phase. WPF rebuilds the full vector scene at a stable maximum of 60 frames per second while DWM continues presenting at each monitor's native cadence, preventing 144–240 Hz panels from multiplying vector/particle CPU work.

## Window motion and lifecycle

- New first-party windows remain transparent through first placement, then use an opacity-only reveal. No top-level STRATA app scales or translates into place.
- Tiled workspace travel retains the directional slide and adds outgoing/incoming crossfades. Desktop-view geometry changes fade cached STRATA content completely away before resize and reveal the live settled surface near the destination, preventing temporary oversized text.
- Closing a tiled compatible window fades exactly that HWND before the existing single-window close/focus handoff. Floating windows remain immediate and do not inherit tiled fades.
- The active-window signal stays hidden while geometry changes and requires stable bounds for at least two composition frames and 70 ms before reappearing.
- Defers automatic activation during presentation mode and full-screen Direct3D states without letting Windows' generic busy state block the idle timer forever. The Screensaver Settings pane and STRATA Command catalog provide manual preview routes.
- Mouse movement, mouse buttons, wheel input, touch/stylus input, or any keyboard key closes the surface. The triggering launch chord is swallowed without leaving Super or Shift logically held, so keybinds resume immediately.
- Preserves Windows screen-saver, display-off, sleep and managed-lock timeouts. Windows policy takes precedence over the STRATA visual screensaver; its status is read-only.

## Wallpaper and theme engine

- Dark, light, and automatic appearance modes.
- Separate `WallpapersDark` and `WallpapersLight` libraries.
- Instant previous/next and collection switching with animated transitions. The active image plus three neighbors in each direction are prepared off-thread with wraparound, and image, palette, widget colors, and monitor frost commit together before the crossfade begins.
- Palette extraction from the active wallpaper.
- Automatic light/dark selection when Auto is enabled.
- Theme propagation to shell surfaces, the click-through active-window signal frame, native inactive borders, charts, widgets, Command Prompt, PowerShell, Windows application-theme preference, and compatible apps' native DWM caption/background/text colors.
- Adjustable vibrancy, glass transparency, continuous backdrop frost, bloom, animation speed, high contrast, and reduced motion. A bounded off-thread cache pre-blurs the active wallpaper neighborhood once per monitor/strength and aligns it beneath first-party surfaces and widget cards. Completed frost textures apply synchronously during the wallpaper presentation transaction. STRATA surfaces and widgets have independent enable toggles driven by the same strength slider; DWM stays clear, so blur does not alter tint opacity or jump among fixed system materials.
- Searchable, paginated wallpaper chooser with thumbnails and a large preview.
- Wallpaper selection and themed confirmation dialogs center in the relevant monitor work area instead of inheriting top-rail or side-panel geometry; confirmations remain modal and above application windows.

The official Stable installer bundles the two approved variants under `wallpapers/`: `WallpapersLight/STRATA_LOGO_LIGHT.jpg` and `WallpapersDark/STRATA_LOGO_DARK.jpg`, with `assets/wallpapers/strata-signal.png` as fallback. Lightweight Preview updates preserve the installed library and do not package wallpapers.

## Desktop widgets

All first-party widgets inherit the live wallpaper palette, opacity, vibrancy, optional wallpaper-aligned blur, bloom, and unified UI scale. Every widget has a top-right Fluent gear for its own behavior, while the main Settings surface separates layer/hover behavior above the individual module switches and keeps shared material controls under Interface.

- Weather with location search, Fahrenheit/Celsius choice, themed or normal icon styles, and expanded forecast details.
- Local time with calendar expansion and a long-form date such as `Sunday, August 30, 2026`.
- Focus timer with configurable duration, progress signal, remaining percentage, pause/reset, quick `+5 min`, completion notification, and reminder text.
- Performance telemetry with CPU/memory color keys matching their graph signals, network, process counts, expanded process management, and close/end-task actions.
- WASAPI spectrum sourced from the active Windows output mix or default microphone, with band/mirror/level rendering, sensitivity control, and a compact previous/pause/play/next strip that directly controls the embedded YouTube player and falls back to system media transport.
- AI CLI panel with one-click provider selection for Antigravity (`agy`), Codex, and Claude, streaming conversation output, stop/reset actions, and working-directory selection.
- YouTube search/player hosted locally in WebView2 with concise status copy, centered playback, optional JPEG previews, autoplay/speed controls, browser account/home routes, and uninterrupted audio/video while the card yields space to another expanded widget.

All widget title lines can be hidden globally without removing their controls or live status content. Widget columns use layout-aware scaling so 720p and 1080p displays keep the complete card stack inside the desktop instead of clipping lower content.

The AI CLI executables are not bundled. YouTube account authentication stays in the default browser rather than the embedded player, and playback depends on YouTube's embeddable-player availability.

## Application discovery and launch

- Desktop and Start Menu shortcuts.
- Registered executable applications.
- Packaged/MSIX applications.
- Direct launchers for STRATA Terminal, STRATA Text, STRATA Snip, external PowerShell, the default browser, STRATA Files, Settings, and ChatGPT.
- Browser launch requests force a new browser window rather than opening only a new tab.
- Search results receive keyboard focus immediately; the first Down key selects the first result, Enter launches it, and a single click launches a result.
- New application windows are focused and assigned to Center Stage as soon as their top-level window becomes manageable.

`Super + Enter` opens STRATA Terminal with a persistent CMD session. See [First-party applications](FIRST_PARTY_APPS.md) for the editor, capture, terminal, and file-selection contracts.

Discovery cannot guarantee launch metadata for portable software that creates no shortcut or registration entry.

## STRATA Files

- Explorer-independent navigation and rendering.
- Known Folder resolution for redirected, OneDrive, and network-backed user folders.
- Home, Desktop, Downloads, Documents, Pictures, Music, Videos, Wallpapers, and mounted volumes.
- Clearly labeled editable path navigation, Back, Forward, Up, Refresh, and history.
- Debounced search/filter field and optional hidden-file display.
- Persistent Details and virtualized Icon views; Name, Kind, Size, Location, and Modified/Deleted headers sort when clicked, and large folders are enumerated off the UI thread and committed in one batch.
- Typing while the file surface is focused selects the first matching filename; repeated letters cycle matches without taking focus from navigation.
- Icon thumbnails prioritize visible, selected, and newly changed files and appear individually as soon as each provider completes.
- Common image previews use a fast direct decoder; videos, PDFs, and codec-backed formats use the registered Windows thumbnail provider.
- STRATA-owned open, save, folder, import, export, wallpaper, script, terminal-output, and working-directory actions use the full themed STRATA Files selection mode rather than Windows Explorer's file dialog.
- Multi-select, keyboard navigation, single/double activation behavior, and drag/drop.
- New folder, rename, cut, copy, paste, copy path/full path, properties, and move to Recycle Bin.
- Native compressed archive (.zip, .rar, .7z, .tar, .gz, .tgz, .bz2) browsing and extraction without spawning Windows Explorer; virtual folder navigation, Up/Back/Forward traversal, on-demand temp cache extraction for opening contained files, Extract All, Extract Here, Extract to Subfolder, and Extract Selected items with progress reporting.
- Native STRATA Recycle Bin view across local drives with original names/locations, deletion timestamps, Restore, permanent Delete, and Empty Recycle Bin actions.
- File-system watcher updates and visible error/status reporting.
- The active location is highlighted in the Places rail.
- The hidden-files toggle sits beside the preview-thumbnail toggle, keeping view controls together.
- Compact-height chrome reduces title, location, command, and footer rows on ScreenPad-style displays so the window fits without requiring fullscreen.
- Dark/light context menus and an icon toolbar.

STRATA Files is intentionally still a focused file manager. Windows shell extensions, library authoring, advanced ACL editing, search indexing, and every Explorer namespace are not yet reproduced.

## Hardware and session controls

- Output and microphone selection, levels/mute, per-app audio session mixer and media transport, with shared compact sound tray controls.
- Native display arrangement, resolution, refresh, orientation, primary/enabled outputs and Extend/Duplicate, protected by a separate timed rollback helper. DPI remains read-only; brightness is available when the hardware exposes a supported interface.
- Native mouse/keyboard and supported Precision Touchpad settings; launcher-selected shell startup apps; power-plan timeouts and battery information; timezone, clock synchronization/manual time and a tray clock/calendar. See [current validation and limitations](STATUS.md).
- Wi-Fi, wired-LAN, and offline link detection with distinct top-rail states.
- Native Windows Wi-Fi lists, radio on/off, saved-profile removal, supported open/WPA2/WPA3 Personal/OWE connection, secret entry, cancellation and adapter-specific disconnect in the clickable network rail panel and Settings. Hidden/enterprise/unsupported enrollment uses Windows-managed profiles; permission denial is reported explicitly.
- Streaming Bluetooth Classic/LE discovery cards, native custom pairing, connection requests, disconnect and removal in Settings and a dedicated Bluetooth rail button. Supported pairing ceremonies cover confirmation, keyboard PIN display, PIN entry and matching-code confirmation.
- Output-device selection is available in the top-bar audio panel, Settings, and the expanded spectrum widget.
- Lock, sleep, hibernate, sign out, restart, and shutdown; destructive actions require confirmation. A completed in-app update offers **Log Off Now** or **Later** so the new immutable release can be activated without hunting for the session controls.
- A confirmed Explorer Session closes STRATA Shell and launches the normal Windows desktop only for the current login while preserving STRATA Shell as the next sign-in shell. Permanent Explorer restoration remains isolated under Settings → Windows Tweaks and the emergency recovery route.
- The power panel omits the redundant Battery + Power text block and places every session/power action icon inside a consistent bordered control.
- STRATA Shell owns ordinary network, Bluetooth, and audio controls without requiring Explorer. Windows performs Bluetooth authentication and tells STRATA which supported custom-pairing prompt to present. UAC, protected device-access consent and drivers remain Windows-owned.

## Windows lock and sign-in wallpaper

- Every successful STRATA wallpaper selection is mirrored asynchronously to the current user's Windows lock screen by default.
- The source image is staged under `%LOCALAPPDATA%\StrataShell\LockScreen` with a fresh Windows-compatible name; JPEG/PNG files are copied and other supported STRATA image formats are converted to PNG.
- Rapid wallpaper changes are coalesced so an older request cannot overwrite the newest selection.
- Windows can reuse the resulting lock-screen image on the sign-in screen when **Show the lock screen background picture on the sign-in screen** is enabled.
- **Windows Tweaks → Sync Lock + Sign-In Wallpaper** can stop or resume future synchronization. Disabling it does not unexpectedly replace the current Windows lock-screen picture.
- STRATA Shell uses current-user personalization APIs and does not force machine-wide lock/logon policy.

## Dialogs and confirmations

- STRATA-owned confirmations, failures, self-test reports, task termination, file recycling, shell changes, and power actions use a wallpaper-theme-aware dialog surface.
- Secure desktop, UAC, driver consent, and hardware pairing ceremonies can still display protected Windows UI because ordinary applications cannot replace those trust surfaces.

## Top rail

Each module can be independently enabled in Settings. The rail reflows when modules are hidden.

- Workspaces
- Context label
- Optional passive CPU signal (telemetry only; it does not open a redundant click/hover panel)
- Active-app icon and name
- Application-tray down-chevron first in the right-side status group
- Network
- Bluetooth
- Audio
- Direct ChatGPT / Codex launch
- Command launcher
- Settings
- Power/session

The tray dropdown groups registrations by product/company identity. Each base application appears once, while distinct publisher controls remain available in its native-menu submenu. Windows hardware/Bluetooth carrier registrations are filtered from the application tray. Apps such as Steam that close their window but remain alive in the notification area no longer retain a Center Stage slot. Background processes without a published tray icon can be shown or explicitly exited after confirmation.

Tray Open restores an existing app window, including a minimized window on another workspace. With no window, it exposes the app's native controls and reopen action. It preserves tray-only menus and supports click-only publishers through a guarded fallback. The speaker button opens device/volume/microphone controls; muting is an explicit button within that panel. Saved expanded widget locks are restored as initial layout, without a hover transition.

## Display continuity and fullscreen games

- Display reconnection and DPI changes retain widget hosts, timers and AI sessions, restore per-display workspace preferences and keep migrated windows reachable.
- Settings replaces its sidebar with compact navigation on narrow work areas; common dialogs relax minimum sizes and keep actions accessible.
- Fullscreen games remain opaque and keep their native geometry. Surround, active border, rail/dock, widgets and ordinary shell notifications yield to fullscreen, including during settings changes. Suppression follows a game between monitors and covers all displays it spans. Shell chrome returns when fullscreen ends or the game is minimized.
- Windows background components are no longer periodically terminated by STRATA. Windows power and lock preferences remain under Windows control.

## Safety and recovery

- Safe preview mode does not change the configured Windows shell.
- The comprehensive self-test validates adaptive 720p–4K layout policy, core layout/frame-clearance policies, composition-synchronized mixed-window transitions, settled tiled crossfades, animation-time material suspension, coalesced non-showing floating placement, delayed active-window signal geometry, wallpaper/frost presentation handoff, palette-wide bloom, repeatable directional and current-workspace multi-monitor routing, dynamic per-monitor workspace safety, widget icon policies, floating-window occlusion/lifecycle, foreground-modal isolation, monitor-centered transient surfaces, STRATA Files operations/type selection/compact-height layout/Recycle Bin metadata, image fit policy, shared picker policy, Terminal persistence, Text encoding/atomic save, Snip capture/encoding, keybinding parser/customization, launcher priority, tray aggregation, Bluetooth physical-device grouping, Windows-edition activation, native caption theming, media compatibility, 60 Hz screensaver render budgeting, private-update safety, lock-screen synchronization, required files, and machine-dependent integrations.
- Installer publishes immutable timestamped releases beneath `%LOCALAPPDATA%\Programs\StrataShell\releases`.
- Windows 11 Home/Pro use a reversible current-user custom-shell policy. Supported Enterprise/Education/IoT editions use Windows Shell Launcher, retain `explorer.exe` as its default fallback, and assign STRATA Shell only to the current user SID.
- Bootstrap starts a temporary Explorer session after three unexpected exits within ten minutes, preserving the selected next-login shell, and provides an independent, user-controlled warning for sustained UI hangs.
- `Ctrl + Alt + Shift + Delete` exits preview or requests Explorer restoration in installed shell mode.
- External recovery scripts and a Start Menu recovery shortcut remain available outside the shell process.

See [Installation and recovery](INSTALLATION.md) before enabling default-shell mode.

## Current technical boundaries

- STRATA Shell is a shell and window orchestrator, not a replacement for the Windows kernel, DWM, security desktop, driver stack, or application frameworks.
- Third-party transparency is whole-window opacity. STRATA Shell cannot transform an unrelated application's internal controls into native acrylic.
- True Compiz-style mesh deformation of third-party pixels is not implemented. First-party shell surfaces use lightweight spring motion, scale, opacity, glow, blur, and bloom.
- Native caption-button hiding is best effort. Some applications redraw or depend on their own chrome.
- Microsoft Media Player is constrained to one primary stage surface and kept opaque for hardware-video compatibility; owned playback/control surfaces do not become separate workspace apps.
- Secure Windows surfaces remain Windows-owned by design.
- Multi-monitor routing and per-monitor view-state policy are implemented and covered by deterministic checks; physical acceptance on a multi-monitor machine remains pending.
- The project is alpha software and does not currently ship Authenticode-signed executables; Batch 1 update archives have a separate mandatory STRATA signature. Automatic updates read the public binary-only release repository; source remains private.

## Batch 1 safety and trust

Signed update inventories, mandatory GitHub digests, verified CRX3 with native consent, an encrypted manual password vault with Windows Hello reveal/copy, safer files/archives, atomic image saves, correct display-rebuild close ownership, and honest missing-telemetry states are implemented in Preview source. See [current validation and limitations](STATUS.md).

## Phase 2 local candidate

Shared background metrics and spectrum capture, bounded wallpaper/frost caches and image decoding, compact Paint undo history, asynchronous image operations, inactive-tab memory targets, explicit browser failure recovery, and bounded asynchronous diagnostics are implemented. The [current validation and limitations](STATUS.md) separates local checks from pending laptop and native browser validation.

## Phase 4 app polish (local candidate)

Interactive ConPTY terminal with bounded VT rendering; cancellable file copies and archive extraction; asynchronous/conflict-aware Text saves; Snip snapshot processing and unsaved-change protection; orientation-correct image loading/export; real shared audio visualization; per-site browser permission review; live download progress/cancellation; process identity checks; and compact/live-theme/accessibility corrections. Open/Save dialogs no longer retain the covered app's active highlight. Implementation and test boundaries are tracked in [current validation and limitations](STATUS.md).

Repeated workspace shortcuts preserve the selected monitor after Up/Down monitor navigation, including empty destinations in tiling mode. Task Manager connects Windows CPU clock, disk activity/transfer rates and per-adapter GPU engine/memory counters. NVIDIA driver temperatures and optional running CPU sensor providers supply real temperatures with source labels. Unsupported measurements remain explicit; no additional sensor driver is installed.
