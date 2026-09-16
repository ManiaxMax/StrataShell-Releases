<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.8

Stable promotion of the accepted Preview window-transition and active-border work. This release keeps the active window clearly identified after movement settles without allowing the border to stretch, lag, or remain inside a transitioning window.

## Settled window presentation

- Coordinates desktop-view, workspace, fullscreen, slot-swap, open, close, move, and resize transactions before restoring settled visuals.
- Keeps native and first-party STRATA windows hidden from stale intermediate frames while their final geometry is established.
- Preserves smooth tiled-window fades and delays the active highlight until the destination window has settled.
- Prevents first-party glass surfaces from carrying stale wallpaper captures or transiently scaling their text during movement.

## Reliable active window border

- Rebuilds the active border as a one-shot, final-size compositor surface instead of resizing and reusing an old transparent window.
- Retires and verifies destruction of the previous border surface before creating its replacement.
- Suppresses the border before layout, fullscreen, workspace, floating resize, and close mutations, including app-owned close actions.
- Uses physical per-monitor geometry and recreates the surface after DPI or monitor changes.
- Keeps the border available after settlement while preventing centered boxes, vertical lines, stale dividers, and delayed outline snaps.

## Stable defaults

- Retains the approved privacy-safe fresh-install baseline: 1.15 vibrancy, 50% glass, 50% Frosted Glass Blur, 40% Bloom, 80% eligible-app opacity, shadows off, and a 7 px / 50% surround.
- Starts STRATA Files in Details view with hidden files and thumbnail previews enabled.
- Enables the active theme border, themed native title bars, Windows theme and lock-screen synchronization, the screensaver, and primary-display widgets.
- Keeps personal wallpaper paths, weather location, device selections, histories, credentials, and machine-specific state out of the package.

## Complete Stable package

- `StrataShell-Setup-1.0.8-win-x64.exe`: full self-contained Setup and uninstaller with the compatible .NET runtime included.
- `StrataShell-1.0.8.zip`: complete self-contained updater and portable bundle with the compatible .NET runtime included.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Existing installations retain their personal settings; only fresh profiles receive the approved portable defaults.
