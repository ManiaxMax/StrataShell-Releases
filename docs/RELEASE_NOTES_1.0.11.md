<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.11

Stable promotion introducing the Full Floating Desktop Environment & Persistent Dock (STRATA-035), direct Media Transport & YouTube gesture input routing (STRATA-036), expanded 8-slot widget deck with Notes widget, Precision Touchpad Gestures, PerMonitorV2 DPI scaling, and Archive support in STRATA Files.

## Floating Desktop Environment & Persistent Dock (STRATA-035)

- **Tiled and Floating Desktop Switching**: Smoothly switch between Center Stage tiling and the new Floating desktop mode (`Super + Shift + D`).
- **Interactive Persistent Dock**: Centered bottom dock featuring theme-aware STRATA icons, pinned technical applications, active window status indicators, and upward-docked flyouts.
- **Floating Window Management**: Freeform window placement, seamless drag-to-float (`Super + Ctrl + Drag`), edge and corner resizing, native window controls, and mode-aware Alt+Tab application switcher.
- **Dock & Desktop Interactions**: Desktop-click to minimize and restore visible windows with clean selection clearing, and contextual OSD positioning (top for Floating, bottom for Tiled).

## Media Controls, YouTube Gestures & Widget Input (STRATA-036)

- **Direct Media Transport Controls**: Resilient native input dispatching for Spectrum widget transport buttons (Previous, Play, Pause, Next) with complete click transaction guarantees.
- **Embedded YouTube Player Input**: Chromium composition host pointer transparency enables reliable direct play/pause clicks, seek gestures, and timeline navigation.
- **Passive Input Demotion Isolation**: Deferred passive-button demotion keeps pointer messages synchronized through mouse-down and mouse-up without losing window activation.
- **Stabilized Hover Handoffs**: Per-column versioned hover targets eliminate rapid widget-to-widget hover flutter and suppress reflow-generated hit-test events.
- **Expanded Widget Deck & Notes Widget**: Expanded to 8 configurable slots per column with socketable widget assignments and a new technical Notes widget.

## Multi-Monitor, DPI & Hardware Integration

- **Precision Touchpad Gestures**: Hardware touchpad workspace navigation with configurable swipe directions.
- **PerMonitorV2 High-DPI Scaling**: Declared PerMonitorV2 manifest ensuring sharp rendering, accurate frame geometry, and unified multi-monitor snipping overlays.
- **Cross-Monitor Directional Movement**: Seamless directional window moves and workspace transitions across mixed-DPI displays.
- **Wallpaper Fit Policies & Screensaver**: Native Fill, Fit, and Stretch wallpaper modes, plus frosted wallpaper integration in the STRATA screensaver.
- **Enhanced Tray Management**: Publisher-grouped background application trays with direct process termination and activation filtering.

## STRATA Files Archive Integration

- **Archive Exploration & Extraction**: Native browsing and extraction of archive formats (.zip) with asynchronous progress reporting and cancellation.

## Complete Stable Package

- `StrataShell-Setup-1.0.11-win-x64.exe`: Full self-contained Setup and uninstaller with the compatible .NET runtime included.
- `StrataShell-1.0.11.zip`: Complete self-contained updater and portable bundle with the compatible .NET runtime included.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Existing installations retain personal settings; only fresh profiles receive the approved portable defaults.
