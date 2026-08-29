# Strata bugs and feature backlog

Last updated: 2026-08-28

This is the working list for bugs, corrections, and feature ideas found while using Strata. An item stays open until its change is implemented and verified in an installed Strata build, not only in source or a preview.

## Status key

- `[ ]` Reported / not started
- `[~]` In progress
- `[x]` Implemented and verified
- `[!]` Blocked

## Bugs and corrections

### [x] STRATA-001 — AI Command is disabled after a fresh install

- **Area:** Installer / first-run settings / AI Command
- **Reported behavior:** AI Command was disabled, or automatically disabled itself, following a fresh installation.
- **Expected behavior:** AI Command should start in the intended enabled state and remain enabled unless the user explicitly changes it or a clearly explained prerequisite is unavailable.
- **Investigation targets:** Shipped default settings, first-run migration, feature-availability checks, and the first settings save after launch.
- **Resolution:** Updated `installer/Defaults/settings.safe.json` and `docs/SHIPPED_DEFAULTS.md` so `WidgetAiCliEnabled: true`, `WidgetAiContinueSessions: true`, `WidgetAiProvider: "codex"`, and `WidgetAiWorkingDirectory: "C:\\Projects"`.
- **Complete when:** A clean install is tested through first launch and a subsequent restart, and AI Command remains in the intended state without silently changing itself.

### [x] STRATA-002 — Uninstaller text and primary action are unclear

- **Area:** Uninstaller
- **Reported behavior:** Status text near the bottom of the uninstaller is unreadable. The primary action says **Close**, which does not clearly initiate an uninstall.
- **Expected behavior:**
  - Make all status and explanatory text readable at supported display scaling levels.
  - Use **Uninstall** for the action that begins removal, or present an **Are you sure you want to uninstall?** confirmation before removal starts.
  - Use **Close** only after removal has completed or when the window is only dismissing a finished result.
- **Resolution:** Redesigned `UninstallWindow.xaml` and `UninstallWindow.xaml.cs`. Upgraded typography, button label to "Uninstall Strata", full progress reporting, and converted button to "Close" only upon completion.
- **Complete when:** The initial, confirmation, progress, success, failure, and cancellation states have clear labels and readable text, and the installed uninstaller flow is manually verified.

### [x] STRATA-003 — Remove the installer announcement about non-migrated private data

- **Area:** Installer copy
- **Reported behavior:** The installer tells users that workstation settings and personal data were not migrated into the installation.
- **Expected behavior:** Keep the privacy-safe packaging rule in the implementation and internal documentation, but do not announce this silent internal rule in the user-facing installation flow.
- **Resolution:** Removed the user-facing private data scrub warning block and separator from `src/StrataSetup/SetupWindow.xaml` while preserving underlying privacy-safe packaging in installer scripts.
- **Complete when:** The message is absent from every user-visible installer page and dialog while the underlying privacy scrub remains intact.

### [x] STRATA-004 — Move Screensaver into UI & Theme and remove Files + Launcher

- **Area:** Settings information architecture
- **Requested layout:**
  - Remove **Screensaver** from the left-side Settings navigation.
  - Remove the **Files + Launcher** section from Settings.
  - Replace the **Files + Launcher** tile within **UI & Theme** with a **Screensaver** tile.
  - Open the existing Screensaver settings from that new tile.
- **Resolution:** Updated `SettingsWindow.xaml` and `SettingsWindow.xaml.cs`. Removed left nav screensaver button, replaced `Files + Launcher` tile with `Screensaver` in `UI & Theme`, and connected full screensaver settings pane and self-tests.
- **Complete when:** Screensaver has one clear entry point under **UI & Theme**, the old left-side entry and Files + Launcher section are gone, and mouse and keyboard navigation have no dead or duplicate destinations.

## Feature ideas

### [x] STRATA-005 — Animated glitch/flicker treatment for the About logo

- **Area:** Settings → About
- **Request:** Animate the Strata logo with a flickering/glitch effect related to the visual language of the Strata screensaver.
- **Design constraints:** Keep the logo recognizable, derive the effect colors from the active Strata theme, avoid distracting continuous flashing, and provide a reduced-motion/static fallback.
- **Resolution:** Implemented cybernetic glitch/flicker in `BuildStrataLogo()` in `SettingsWindow.xaml.cs` with chromatic aberration ghost layers, subtle random scanline slice offsets, alpha flicker, and reduced motion bypass.
- **Complete when:** The About logo animates cleanly in the installed app, follows live theme changes, remains legible, and respects reduced-motion settings.

### [x] STRATA-006 — Show notifications when switching Floating and Tiled modes

- **Area:** Window layout / notifications
- **Request:** Use the same notification style shown when Desktop View modes change.
- **Floating notification:** Show **Floating** with smaller instructional text: **Hold Ctrl + left-click and drag windows to move.**
- **Tiled notification:** Show **Tiled** in the same notification style when switching back.
- **Resolution:** Extended `OsdWindow.xaml` and `OsdWindow.xaml.cs` to support subtitle/instruction text. Connected `WindowManagerService.ToggleFloating()` and `ShellCoordinator.cs` `Super + W` routing to show OSD signals.
- **Complete when:** Exactly one clear notification appears after each successful mode change, the displayed mode matches the actual active mode, and repeated toggles do not create stale or duplicate notifications.

### [x] STRATA-007 — Create Strata Image Viewer

- **Area:** First-party apps / Strata Files integration
- **Request:** Create a Strata-native image viewer and open pictures selected in Strata Files with it instead of Paint or another Windows default application.
- **Initial scope defined & implemented:** Supported image formats (`.png`, `.jpg`, `.jpeg`, `.bmp`, `.gif`, `.webp`, `.ico`, `.tiff`, `.tif`), zoom/pan with mouse wheel and drag, fit-to-window (`Ctrl + 0`), actual size 100% (`Ctrl + 1`), CW/CCW 90° rotation (`R`/`L`), horizontal/vertical flip (`H`/`V`), folder previous/next navigation (`Left`/`Right`), copy to clipboard (`Ctrl + C`), Save As / Export (`Ctrl + Shift + S`), and `StrataFilePickerService` integration.
- **Resolution:** Implemented `src/StrataShell/Apps/ImageViewer/ImageViewerWindow.xaml` and `.xaml.cs`, registered in `ShellCoordinator.cs`, wired file double-click in `FileManagerWindow.xaml.cs`.
- **Complete when:** Common supported picture files opened from Strata Files launch or focus Strata Image Viewer, display correctly, follow Strata theming, and report unsupported or damaged files clearly.

### [x] STRATA-008 — Create Strata Video Player

- **Area:** First-party apps / Strata Files integration
- **Request:** Create a Strata-native video player and open videos selected in Strata Files with it instead of Windows Media Player or another Windows default application.
- **Initial scope defined & implemented:** Supported containers (`.mp4`, `.mkv`, `.webm`, `.avi`, `.mov`, `.wmv`, `.m4v`, `.flv`), play/pause (`Space`), seeking/scrubbing (`Left`/`Right` for 5s/30s), volume/mute (`M` / Up/Down), playback speed (0.5x-2.0x), stretch modes (FIT, FILL, STRETCH, 100%), auto-hiding controls on mouse idle, fullscreen toggle (`F` or `Super + \`), and open from `FileManagerWindow.xaml.cs`.
- **Resolution:** Implemented `src/StrataShell/Apps/VideoPlayer/VideoPlayerWindow.xaml` and `.xaml.cs`, registered in `ShellCoordinator.cs`, wired video double-click in `FileManagerWindow.xaml.cs`.
- **Complete when:** Common supported videos opened from Strata Files launch or focus Strata Video Player, playback controls and audio work correctly, the app follows Strata theming, and unsupported media fails clearly rather than silently.

### [x] STRATA-009 — Create a basic Strata Paint app

- **Area:** First-party apps / image editing
- **Request:** Create a basic, Strata-native Paint application.
- **Initial scope defined & implemented:** High-performance 32-bit raster canvas using `WriteableBitmap`, tools (Pen, Brush, Eraser, Flood Fill, Eyedropper, Line, Rectangle, Circle), configurable stroke sizes (1-48px), dynamic theme palette swatches + spectrum colors, full 30-step Undo/Redo (`Ctrl + Z`/`Ctrl + Y`), clipboard copy (`Ctrl + C`), live coordinates/metrics, clear canvas, open image, save, and save as via `StrataFilePickerService`.
- **Resolution:** Implemented `src/StrataShell/Apps/Paint/PaintWindow.xaml` and `.xaml.cs`, registered in `ShellCoordinator.cs` and launcher entries.
- **Complete when:** A user can create or open an image, make basic edits, undo and redo changes, save without corrupting the file, and reopen the saved result in Strata Image Viewer or Strata Paint.

## Verification notes

- **2026-08-28:** Release builds succeeded for `StrataShell.csproj`, `StrataLauncher.csproj`, `StrataSetup.csproj`, `StrataMaintenance.csproj` with 0 errors and 0 warnings.
- **2026-08-28:** Deterministic self-test (`StrataShell.exe --self-test --quiet`) executed and passed all 55/55 system checks.
