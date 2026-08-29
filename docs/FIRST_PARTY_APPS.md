# First-party applications

Strata Terminal, Strata Text, Strata Snip, Strata Image Viewer, Strata Video Player, and Strata Paint are shell-native applications. They use the same live wallpaper palette, dark/light decision, acrylic material, opacity, blur, bloom, shadow, active-border, workspace, and window-presentation services as Settings and Strata Files. `Super + T` toggles the active first-party application's transparent material without changing another keybinding.

## Shared Strata Files contract

All first-party user-facing file routes open the complete `FileManagerWindow` in one of three modes:

- **Open** selects an existing file and applies the caller's extension filter.
- **Save** validates the filename, adds the preferred extension when omitted, and uses a themed overwrite confirmation.
- **Select Folder** returns the current folder or a selected child folder.

The shared route is used for text open/save, screenshot open/export, terminal script/output, startup-manifest import/export, AI CLI working folders, wallpaper browsing, image/video playback, paint exports, and other Strata-owned load/save workflows. It remains Explorer-independent and inherits Strata Files search, path entry, Places, Details/Icon views, hidden-file behavior, and thumbnails.

## Strata Terminal

- `Super + Enter` opens the integrated terminal with a persistent CMD session.
- The profile switcher can start persistent CMD or PowerShell sessions without replacing the window.
- Command history, streamed standard/error output, current-directory tracking, script loading, output export, stop/reset, and working-directory selection are built in.
- `Super + Ctrl + Enter` remains the external PowerShell shortcut.
- Elevation starts an isolated `--terminal-only` Strata process so an administrator terminal never starts a second desktop shell.

## Strata Text

- Plain-text and code editing with new/open/save/save-as routes through Strata Files.
- UTF-8, UTF-16 LE/BE, BOM, CRLF/LF, dirty-state, and atomic replacement behavior are preserved across a load/save cycle.
- The editor is workspace-managed and follows live theme/material changes.

## Strata Snip

- `Super + Ctrl + S` opens the full capture utility.
- The top-rail capture icon skips the utility landing state and immediately opens rectangle selection.
- Rectangle, selected-window, and complete virtual-desktop capture account for multi-monitor coordinates.
- The editor supports pen/highlighter/erase markup, clipboard copy, image open, and PNG/JPEG export through Strata Files.
- Capture overlays are temporary topmost tool surfaces; the resulting editor returns to ordinary Strata workspace management.

## Strata Image Viewer

- High-performance, GPU-accelerated image viewer for `.png`, `.jpg`, `.jpeg`, `.bmp`, `.gif`, `.webp`, `.ico`, `.tiff`, and `.tif`.
- Smooth pan and zoom (mouse drag and wheel zoom), fit-to-window (`Ctrl + 0`), actual size 100% (`Ctrl + 1`), CW/CCW 90° rotation (`R` / `L`), horizontal/vertical flip (`H` / `V`).
- Directory traversal with `Left` / `Right` arrows and page navigation.
- Clipboard copy (`Ctrl + C`) and export through Strata Files (`Ctrl + Shift + S`).

## Strata Video Player

- Clean, themed video player supporting `.mp4`, `.mkv`, `.webm`, `.avi`, `.mov`, `.wmv`, `.m4v`, and `.flv`.
- Play/pause (`Space`), seeking (`Left` / `Right`), volume/mute (`M`, `Up` / `Down`), speed selector (0.5×–2.0×), stretch mode cycles (FIT, FILL, STRETCH, 100%), and auto-hiding playback controls during playback.
- Fullscreen and glass-expanded mode (`F` or `Super + \`).

## Strata Paint

- 32-bit raster drawing canvas with direct pixel manipulation using `WriteableBitmap`.
- Tools: Freehand Pen, Brush, Eraser, Flood Fill, Eyedropper, Line, Rectangle, and Ellipse.
- Configurable stroke sizes (1–48px) with keyboard shortcuts (`[` / `]`).
- Theme-derived dynamic palette swatches, 30-step Undo/Redo (`Ctrl + Z` / `Ctrl + Y`), clipboard copy (`Ctrl + C`), and Open/Save/Save As through Strata Files.

## Floating interaction

`Super + W` places every app on the active workspace into floating mode. While floating, hold `Ctrl` and left-drag anywhere inside a window to move it, including an app with no title bar. The active float hides only the peer windows and widget cards it physically covers; those surfaces return as soon as the overlap clears. Tiled, glass-expanded, and true-fullscreen windows remain locked to their managed geometry.

The top rail follows a separate physical-overlap rule: any managed window crossing its bounds makes the rail fade out and stop intercepting pointer input. Moving the window clear restores it; touching the top edge temporarily reveals the rail while the overlap remains.
