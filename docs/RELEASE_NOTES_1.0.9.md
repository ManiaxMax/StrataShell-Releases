<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.9

Stable promotion of multi-monitor DWM performance optimizations, per-monitor DIP scaling, low-effects rendering mode, active window highlight refinements, and standardized official STRATA branding.

## Multi-Monitor & DWM Performance

- **Multi-Monitor DWM Optimization**: Consolidates edge glows across multi-monitor setups, throttles audio spectrum processing when idle, preserves native DXGI swapchains, and relaxes top rail timers to reduce GPU/CPU overhead.
- **Per-Monitor DIP Scaling**: Adds per-monitor surface DIP scaling and low-effects rendering performance mode to ensure sharp, stutter-free rendering across mixed DPI and multi-display environments.
- **Telemetry Threading & Settle**: Optimizes active window highlight settle timing and offloads telemetry operations to background threads.
- **Process Optimization**: Adds tuning to minimize background memory and CPU usage from idle Windows host processes.

## Official Branding & Identity

- **Standardized STRATA Branding**: Standardizes the official **STRATA Shell**, **STRATA Files**, and **STRATA Terminal** product naming across all first-party surfaces, windows, and documentation.
- **Official Launcher & Logo Assets**: Integrates official high-resolution STRATA logo and icon branding across the shell, installer, and public release documentation.
- **Universal Asset URLs**: Updates release metadata, documentation, and asset links to point to the public distribution repository for reliable rendering across all platforms.

## Stable Defaults & Packaging

- Retains the approved privacy-safe fresh-install baseline: 1.15 vibrancy, 50% glass, 50% Frosted Glass Blur, 40% Bloom, 80% eligible-app opacity, shadows off, and a 7 px / 50% surround.
- Starts STRATA Files in Details view with hidden files and thumbnail previews enabled.
- Enables active theme border, themed native title bars, Windows theme and lock-screen synchronization, screensaver, and primary-display widgets.
- Keeps personal wallpaper paths, weather location, device selections, histories, credentials, and machine-specific state out of the package.

## Complete Stable Package

- StrataShell-Setup-1.0.9-win-x64.exe: Full self-contained Setup and uninstaller with the compatible .NET runtime included.
- StrataShell-1.0.9.zip: Complete self-contained updater and portable bundle with the compatible .NET runtime included.
- RELEASE_MANIFEST.json, VALIDATION_REPORT.json, individual Setup checksum, and unified SHA256SUMS.txt.
- Existing installations retain their personal settings; only fresh profiles receive the approved portable defaults.