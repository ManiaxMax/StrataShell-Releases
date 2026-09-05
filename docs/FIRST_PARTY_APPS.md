# First-party applications

Browser, Files, Text, Paint, Image Viewer, Terminal and Media Player support multiple document windows. Normal launches open a new window; the Floating bar groups them by application with a count, hover/click previews, individual Close and Close all. Shift-click or the bar menu opens another window. Settings and other utility panels keep their existing single-surface behavior. Browser's tab-row utility area contains only Menu; Bookmarks and History sit on the navigation toolbar beside the single Downloads control, with no developer-tools toolbar button.

STRATA Terminal, STRATA Text, STRATA Snip, STRATA Image Viewer, STRATA Media Player, STRATA Paint, STRATA Task Manager, and STRATA Browser are shell-native applications. They use the same live wallpaper palette, dark/light decision, wallpaper-aligned frosted material, transparency, blur, bloom, shadow, active-border, workspace, and window-presentation services as STRATA Settings and STRATA Files. `Super + Shift + T` toggles the active first-party application's transparent material without changing another keybinding.

## Shared STRATA Files contract

In Floating mode, first-party application windows expose minimize/maximize, title-bar dragging, and resizable themed edges through the shared window manager. Their dock icons use the existing 24-unit vector family and dynamic palette brushes. Minimized applications remain reachable in the dock and Alt + Tab; modal file and wallpaper dialogs retain their owner relationship. Returning to Tiled restores each surface's original resize/chrome configuration.

All first-party user-facing file routes open the complete `FileManagerWindow` in one of three modes:

- **Open** selects an existing file and applies the caller's extension filter.
- **Save** validates the filename, adds the preferred extension when omitted, and uses a themed overwrite confirmation.
- **Select Folder** returns the current folder or a selected child folder.

The shared route is used for text open/save, screenshot open/export, terminal script/output, startup-manifest import/export, AI CLI working folders, wallpaper browsing, image/media playback, paint exports, and other STRATA-owned load/save workflows. It remains Explorer-independent and inherits STRATA Files search, path entry, Places, Details/Icon views, hidden-file behavior, thumbnails, mouse marquee drag-select, file drag-and-drop, compressed archive (.zip, .rar, .7z) browsing and extraction, bottom-left operation progress tracking, and interactive column header click-to-sort.

## STRATA Terminal

- `Super + Enter` opens the integrated terminal with a persistent CMD session.
- The profile switcher can start persistent CMD or PowerShell sessions without replacing the window.
- Interactive sessions use Windows ConPTY with a bundled, offline xterm.js renderer. Console programs receive real keyboard input, terminal dimensions and Ctrl+C. Output has bounded backpressure and 3,000 lines of scrollback; reading older output does not force scrolling to the bottom. Ctrl+Shift+C/V copy/paste, Ctrl+Shift+F searches scrollback, and Ctrl+Shift+S exports it. The bottom command field stages scripts before execution.
- The renderer cannot navigate, download, access the network or use host objects. A separate WebView profile avoids browser cookies/extensions. If initialization fails, an explicitly labelled basic command mode retains bounded output; interactive console programs require the renderer. CMD and PowerShell restart/switch, current-directory tracking and script/output pickers remain available.
- Elevation starts an isolated `--terminal-only` STRATA process so an administrator terminal never starts a second desktop shell. After Windows accepts elevation, the integrated source window closes instead of hiding, releases its session and workspace registration, and hands ownership to the elevated terminal. Cancelling Windows confirmation leaves the original session open. The parent shell monitors the elevated process through exit, synthesizes modifier releases, and reinstalls its dedicated low-level keyboard router after both UAC handoff and process closure, preventing a latched Windows-logo key or dead STRATA shortcuts.
- Administrator and policy-bypass actions use the shared code-native shield and policy icons rather than font-dependent symbols.

## STRATA Text

- Plain-text and code editing with new/open/save/save-as routes through STRATA Files.
- Unsaved close offers Save/Discard/Cancel; an approved close is queued after WPF's original close callback returns, preventing Discard from reentering a closing window.
- UTF-8, UTF-16 LE/BE, BOM, CRLF/LF, dirty-state, and atomic replacement behavior are preserved across a load/save cycle.
- Opens and saves run in workers. A newer open, typing, or document switch invalidates stale loads; typing during a save leaves newer edits dirty. A content fingerprint rejects saves over externally changed files. The status format button selects UTF-8/BOM/UTF-16 LE and line endings. Legacy Latin-1 fallback is labelled and rejects characters it cannot represent instead of silently replacing them. Documents are limited to 4 MiB; undo and word-count allocation are bounded.
- The editor is workspace-managed and follows live theme/material changes.

## STRATA Snip

- `Super + S` opens the full capture utility.
- The top-rail capture icon skips the utility landing state and immediately opens rectangle selection.
- Rectangle, selected-window, and complete virtual-desktop capture account for multi-monitor coordinates.
- The editor supports pen/highlighter/erase markup, clipboard copy, image open, and PNG/JPEG export through STRATA Files.
- Captures up to 32 megapixels are processed off the UI thread; oversized virtual desktops are rejected before allocating a capture. Composition and image encoding use frozen snapshots. Unsaved captures and markup offer Save/Discard/Cancel, and edits made during export remain dirty.
- Like Text and Paint, an approved unsaved close is deferred until the original closing callback returns. Canceling either the confirmation or Save picker keeps the capture and markup open.
- Capture overlays are temporary topmost tool surfaces; the resulting editor returns to ordinary STRATA workspace management.

## STRATA Image Viewer

- High-performance, GPU-accelerated image viewer for `.png`, `.jpg`, `.jpeg`, `.bmp`, `.gif`, `.webp`, `.ico`, `.tiff`, and `.tif`.
- Automatic viewport scaling for large and high-resolution images to fit the window on load and resize.
- Smooth pan and zoom (mouse drag and `Ctrl + Scrollwheel` / wheel zoom / in-app `+` and `−` buttons), fit-to-window (`Ctrl + 0`), actual size 100% (`Ctrl + 1`), CW/CCW 90° rotation (`R` / `L`), horizontal/vertical flip (`H` / `V`).
- Directory traversal with `Left` / `Right` arrows and page navigation.
- Clipboard copy (`Ctrl + C`) and export through STRATA Files (`Ctrl + Shift + S`) include rotation and flips at original pixel resolution. EXIF orientations 1–8 are honored on load. Zoom and pan affect the view, not export dimensions. This remains an SDR, 8-bit editing/display path; HDR fidelity is not claimed.

## STRATA Media Player

- Shell-native audio and video player supporting video formats (`.mp4`, `.mkv`, `.webm`, `.avi`, `.mov`, `.wmv`, `.m4v`, `.flv`, `.mpg`, `.mpeg`, `.3gp`, `.asf`) and audio formats (`.mp3`, `.wav`, `.flac`, `.aac`, `.m4a`, `.ogg`, `.wma`, `.aiff`, `.opus`, `.mid`, `.midi`).
- Dedicated audio stage with shared system-output spectrum data, filename/folder labels, and an audio-format badge. The spectrum is labelled as system audio and may include other apps; it does not pretend to be track-isolated analysis. Hidden/minimized/paused/reduced-motion views stop spectrum capture and visual timers. Audio playback can continue while hidden. Folder playlists load in a worker with stale-result rejection and a 10,000-item cap. Decoder failures explain that codec availability depends on Windows.
- Play/pause (`Space`), seeking (`Left` / `Right` for 10s or 30s with `Shift`), track navigation (`PageUp` / `PageDown`), volume/mute (`M`, `Up` / `Down`), speed selector (0.5×–2.0×), aspect ratio stretch mode cycles for video (FIT, FILL, STRETCH, 100%), and auto-hiding playback controls during video playback.
- Fullscreen within the player (`F`) plus shell-managed expansion through `Super + Shift + \`.

## STRATA Paint

- 32-bit raster drawing canvas with direct pixel manipulation using `WriteableBitmap`.
- Canvas zoom from 10% to 1600% via `Ctrl + Scrollwheel`, in-app `+` and `−` toolbar buttons, and `Ctrl + 0` / `Ctrl + 1` reset.
- Tools: Freehand Pen, Brush, Eraser, Flood Fill, Eyedropper, Line, Rectangle, and Ellipse.
- Configurable stroke sizes (1–48px) with keyboard shortcuts (`[` / `]`).
- Theme-derived dynamic palette swatches, 30-step Undo/Redo (`Ctrl + Z` / `Ctrl + Y`), clipboard copy (`Ctrl + C`), and Open/Save/Save As through STRATA Files.

## STRATA Task Manager & Performance Monitor

- `Super + Ctrl + T` opens the shell-native task manager and performance telemetry dashboard inspired by TMOG as a centered floating utility surface (matching Wallpaper Chooser) rather than occupying a tiled Center Stage slot.
- Summary overview dashboard with multi-segment neon VU meters, real-time glowing line graphs with bloom effects, top CPU and top memory processes preview, and mini hardware stat cards.
- Hardware deep-dive view with dedicated tabs for CPU, Physical/Virtual Memory, Storage Disks, Network Adapters, and GPU Accelerators.
- Process selection survives refresh when PID and creation time still match. End/priority actions retain a native process handle and verify the displayed creation time before acting; missing identity or denied access produces an explicit failure.
- Advanced Process Manager with live CPU %, Memory working set, Threads, Handles, Status, and explicit Unavailable fields where access or sampling is missing. Supports search filtering, category filtering (Apps, Background, System), End Task, End Process Tree, Priority adjustments, and Protected Process safety guards.
- Windows Services manager with real-time status filtering (Running, Stopped), Start, Stop, and Restart actions.
- Startup apps catalog with toggle controls; unmeasured startup impact remains unavailable.
- Logical drive space analyzer and system architecture specifications.
- Task Manager uses a single shared glass material when transparency is enabled; cards do not add opaque fills. Covered windows cannot draw their active border above another managed app. The unused Users page is removed.

## STRATA Browser

- `Super + B` opens the shell-native Chromium-based web browser powered by Microsoft WebView2.
- Multi-tabbed browsing with dynamic tab creation (`+` / `Ctrl + T`), tab closing (`×` / `Ctrl + W`), tab cycling (`Ctrl + Tab` / `Ctrl + Shift + Tab`), and direct tab selection (`Ctrl + 1` through `Ctrl + 9`).
- Smart Omnibox with auto-detection of web URLs, local file paths, localhost/IP ports, and instant search fallback with loading progress indicator.
- Live security classification badge: Secure (TLS/HTTPS), Local resource, or Insecure HTTP.
- Bookmarks management with Star toggle (`Ctrl + D`), quick bookmarks toolbar (`Ctrl + Shift + B`), and persistent storage in `%LocalAppData%\StrataShell\browser-bookmarks.json`.
- Automatic browsing history with search filter drawer (`Ctrl + H`) and persistence in `%LocalAppData%\StrataShell\browser-history.json`.
- In-page search (`Ctrl + F`) with match navigation (`Enter` / `Shift + Enter` / `F3`) and real-time DOM highlight.
- Chromium Developer Tools integration (`F12` / `Ctrl + Shift + I`).
- Viewport Zoom controls (`Ctrl + +`, `Ctrl + -`, `Ctrl + 0`) with percentage readout.
- Embedded STRATA Start page (`strata://newtab`) with live theme accenting, instant web search, and developer speed dials.
- Native website prompts identify the requesting origin and capability, with Allow once/Always allow/Deny. Browser Settings → Site permissions reviews and resets individual saved choices or all choices in the current profile; reload pages to end existing access. Downloads show origin and live progress; Cancel remains available while a partial file exists, and Clear preserves active transfers.
- Full workspace management, live theme/material changes, drag move, and `Super + Shift + T` transparency toggle.

## Floating interaction

Hold `Super + Ctrl` and left-click/left-drag anywhere inside a window to float and move it, or drag borders to resize. Hold `Super + Ctrl` and right-click a floating window to restore it to tiled Center Stage mode. The active float hides only the peer windows and widget cards it physically covers; those surfaces return as soon as the overlap clears. Tiled, glass-expanded, and true-fullscreen windows remain locked to their managed geometry.

The top rail follows a separate physical-overlap rule: any managed window crossing its bounds makes the rail fade out and stop intercepting pointer input. Moving the window clear restores it; touching the top edge temporarily reveals the rail while the overlap remains.

## Batch 1 safety behavior

Extensions require verified CRX3 identity and native permission consent; downloads never install automatically. Unpacked developer folders require explicit unsigned-code consent. Manual passwords are encrypted for the Windows account; reveal/copy requires Windows Hello. Unreadable vaults are preserved in read-only mode.

Files captures transfer sources/destination, rejects link/junction ambiguity before recursive transfers and preserves sources after failed cross-volume copies. Archive opening shares bounded extraction and download-origin protection. Paint, Image Viewer and Snip use atomic, format-correct saves; Paint offers Save/Discard/Cancel.

Batch 1 replaced invented Task Manager readings with explicit Unavailable states. Phase 4 subsequently connects Windows-reported CPU clock, logical-disk activity/read/write/latency, GPU engine load and dedicated/shared memory. Counters collect on a worker only while the visible, unpaused monitor is sampling. GPU identity uses DXGI LUIDs; the detail view lets users choose among adapters, with independent histories. GPU load uses the busiest engine after summing processes within that engine; summary disk activity shows the busiest fixed volume. Network history uses MB/s. Metric labels change with the selected hardware page.

NVIDIA GPU temperature uses the installed driver's NVML library from System32. CPU temperature uses an already-running Libre Hardware Monitor or Open Hardware Monitor WMI provider; STRATA does not install a driver or launch that software. Readings identify their source, and unsupported sensors remain unavailable. Identical physical NVIDIA cards with ambiguous model names do not receive an assigned temperature. Network totals exclude addressless Windows filter layers that repeat the parent interface's counters; totals across IP adapters can still include both VPN and underlying transport traffic.

AMD/Intel GPU temperatures, power draw, per-core usage, memory pools/cache, detailed RAM specifications, per-process disk/network/GPU accounting and user-session accounting remain unsupported by this implementation. Startup impact is explicitly not measured. Missing, denied and warming-up counters remain unavailable; they are not reported as idle. See [current validation and limitations](STATUS.md) for the measured host and remaining scope.

## Phase 4 shared polish (local candidate)

Paste and dropped-file copies share within-file progress, cancellation, keep-both collision naming and a completion/failure report. Archive extraction uses the same cancellation control. Completed files remain after cancellation; an incomplete folder can contain completed files, and cross-volume moves preserve remaining sources. Windows I/O that is blocked on a disconnected device/share may not acknowledge cancellation immediately.

Open/Save pickers and owned dialogs suppress the covered parent's active highlight, including delayed highlight callbacks, and restore the owner on dismissal. Generated controls promote known palette brush references to live resources and give tooltip-labelled buttons accessible names. Compact toolbars retain access to their actions through overflow or wrapping. See [current validation and limitations](STATUS.md) for verified evidence and installed/hardware checks still outstanding.

Task Manager collapses its navigation below 1100 DIPs, reflows dashboard cards and stacks system-information cards; graph labels follow the theme. Terminal ANSI colors and the browser start page use theme-matched backgrounds so Light mode remains readable. Settings descriptions follow live theme changes.
