<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.12

Stable promotion introducing Rail Work Area Reservations for maximized windows in Tiled and Floating desktop modes, and comprehensive Fullscreen Game & Application Protection that suppresses shell chrome, edge reveal, and active border overlays during fullscreen sessions.

## Maximized Window Rail & Dock Work Area Bounds

- **Work Area Reservation**: The top rail in Tiled mode and the bottom dock in Floating mode now properly reserve monitor work area through the native appbar protocol.
- **Maximized Boundary Enforcement**: Ordinary maximized windows, both native Win32 applications and borderless STRATA windows, strictly stop at the inner boundary of the rail/dock rather than overflowing underneath.
- **Multi-Monitor Bounds**: Negotiated bounds operate per-monitor, supporting mixed display origins and varying display resolutions.

## Borderless Fullscreen Game & Application Protection

- **Shell Chrome Suppression**: When a game or application enters borderless or exclusive fullscreen, STRATA Shell automatically suppresses the top rail, bottom dock, launcher, and quick panels.
- **Edge Reveal Shield**: Screen edge mouse contact is disabled during fullscreen sessions, preventing accidental reveals of shell chrome over gameplay.
- **Active Border Overlay Suppression**: The topmost active-window accent border overlay is synchronously retired during fullscreen, ensuring games and media retain an unobstructed display.
- **Preserved Opacity**: Fullscreen windows remain fully opaque, suspending window translucency.

## Complete Stable Package

- `StrataShell-Setup-1.0.12-win-x64.exe`: Full self-contained Setup and uninstaller with the compatible .NET runtime included.
- `StrataShell-1.0.12.zip`: Complete self-contained updater and portable bundle with the compatible .NET runtime included.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Existing installations retain personal settings; only fresh profiles receive the approved portable defaults.
