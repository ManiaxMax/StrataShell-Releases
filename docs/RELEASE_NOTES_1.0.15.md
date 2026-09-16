<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.15

Stable release delivering terminal elevation isolation, cold-startup widget slot classification, frosted viewport material transparency, desktop show/hide support, Bluetooth LE timeout guidance, and UI polish.

## Terminal Surface & Elevation Isolation

- **Elevated Terminal Readiness**: Elevated terminals start directly in basic command mode, bypassing WebView2 initialization so administrator command execution remains instant and independent of cross-integrity browser processes.
- **Initialization Boundary**: Interactive terminal sessions bound CoreWebView2 environment and controller initialization to 20 seconds before gracefully falling back to basic command mode.
- **WebView Profile Isolation**: Separates regular and elevated WebView profiles (`TerminalWebView` vs. `TerminalWebViewAdmin`) to prevent integrity conflicts.

## Widget Layout & Cold Startup

- **Saved Slot Assignment**: Resolves widget column classification directly from assigned slot IDs prior to visual tree template attachment, ensuring saved left/right widget positions and expansion states restore accurately on cold launch.
- **Desktop Acceptance**: Validates column orientation and lock preservation before window show across desktop reloads.

## Viewport Transparency & Frosted Materials

- **Frosted Viewport Material**: Enhances web browser and terminal content viewports under frosted blur, revealing desktop wallpaper depth while preserving text legibility.
- **Popup & Control Theming**: Aligns browser download indicators, popup surfaces, and display panels with active live theme palettes.

## System Controls & Desktop Polish

- **Desktop Toggle**: Adds dedicated per-monitor desktop show/hide shortcut (`Super + D`) and interaction handling.
- **Bluetooth LE Guidance**: Clarifies disconnection timeout messages for Bluetooth Low Energy peripherals that remain bound by Windows services.
- **Files Stability**: Fixes potential NullReferenceException during rapid List and Icons view mode switches in STRATA Files.

## Complete Stable Package

- `StrataShell-Setup-1.0.15-win-x64.exe`: Full self-contained Setup and uninstaller with bundled .NET 10 runtime.
- `StrataShell-1.0.15.zip`: Complete self-contained updater and portable bundle with bundled .NET 10 runtime.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Existing installations preserve personal configurations; fresh installations seed the approved Balanced Floating layout.