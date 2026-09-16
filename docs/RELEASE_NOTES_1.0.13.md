<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.13

Stable promotion transitioning STRATA Shell to .NET 10 and introducing native power and battery management, comprehensive native system and wireless settings, floating dock window group previews and minimize animations, Explorer-free monitor work area reservations, and performance-optimized frosted wallpaper rendering.

## .NET 10 Runtime & Foundation

- **Complete .NET 10 Migration**: STRATA Shell, Launcher, Maintenance, and Setup now target .NET 10 with high-performance desktop runtime integration.
- **Stable-Only Self-Contained Packaging**: Stable installers and updater bundles include the complete .NET 10 Windows Desktop runtime (including Windows Forms and Drawing dependencies for system tray compatibility).

## Native Power & Battery Controls

- **Battery Tray Menu**: Dedicated tray flyout reporting battery charge percentage, charging/AC status, available capacity/health, discharge rates, and Windows Energy Saver status.
- **Windows Power Modes**: Native configuration of Best power efficiency, Balanced, and Best performance modes for AC and battery operation.
- **Automatic Power Saving**: Automatically engages economical wallpaper scaling, disables heavy blur/bloom, and caps animations at 60 Hz during battery saving conditions.
- **Battery Usage History**: On-demand aggregate historical active usage metrics without third-party tracking.

## Native System & Wireless Settings

- **Display Configuration**: Interactive monitor arrangement map, resolution, refresh rate, orientation, and primary display selection with automatic preview timeout rollback.
- **Wireless Management**: Native Bluetooth device discovery and PIN pairing, along with Wi-Fi network disconnection, forget, and radio state controls without Explorer dependency.
- **Audio & Time**: Shared output and microphone endpoint selection, per-app session mixer, Windows time server synchronization, and month-navigation calendar.

## Floating Dock & Window Enhancements

- **Window Group Previews**: Hover and group-click thumbnail previews with individual and batch close capabilities.
- **Native Work Area Reservations**: Direct per-monitor `SPI_SETWORKAREA` negotiation ensuring maximized windows respect rail and dock boundaries in Explorer-free environments.
- **Document Windows**: Multi-window support for Browser and Media alongside Files and Text Editor, with shared profile persistence.

## Complete Stable Package

- `StrataShell-Setup-1.0.13-win-x64.exe`: Full self-contained Setup and uninstaller with bundled .NET 10 runtime.
- `StrataShell-1.0.13.zip`: Complete self-contained updater and portable bundle with bundled .NET 10 runtime.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Existing installations preserve personal configurations; fresh installations seed the approved Balanced Floating layout.
