<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.18

Stable release featuring the full self-contained .NET 10 runtime package, comprehensive shortcut remapping, integrated browser download management, and accumulated desktop visual and app improvements.

## .NET 10 Runtime Package

- **Self-Contained Windows x64 Runtime**: Full self-contained installer (`StrataShell-Setup-1.0.18-win-x64.exe`) and portable bundle (`StrataShell-1.0.18.zip`) bundling the .NET 10 runtime (`coreclr.dll`, `hostfxr.dll`).
- **Zero-Dependency Installation**: Runs without requiring pre-installed system runtimes while preserving personal configuration profiles during upgrades.

## Shortcut Customization & Input

- **Universal Shortcut Remapping**: Remap any STRATA shortcut from the interactive editor (`Super + Ctrl + K`). Conflicting bindings, occupied combinations, and physical aliases are rejected.
- **Individual Resets**: Easily restore individual shortcuts to factory defaults with dedicated per-binding reset buttons.

## Browser & File Management

- **Integrated Downloads**: Browser download completion routes directly to STRATA Files; download progress bar visualizes transfer state.
- **Multi-Monitor Rails**: Secondary top rails synchronize theme palettes dynamically across monitors.

## Visuals, Performance & Windows Management

- **Bloom Quality Options**: Configurable Low/High bloom quality levels and selectable bloom scope; High performance preset uses economical bloom with frosting and disabled drop shadows.
- **Continuous Material Frost**: Continuous wallpaper frost and smooth material preservation during workspace slides and window swaps.
- **Floating Snaps & Minimized Previews**: Enhanced floating edge snap previews, minimized-window dock previews, and crisp fullscreen restoration.
- **Hardware & Peripherals**: USB ejection capability detection, Wi-Fi auto-connection options, and wallpaper transition choices.

## First-Party App Revisions

- **Paint & Snip Palette**: Eight quick swatches and interactive visual HSV custom color popup.
- **Paint Selection**: Rectangular selection with move/resize dragging, corner handles, clipboard copy/cut/paste, and background canvas restoration.
- **Text Typography**: Dedicated Notepad-style font dialog with family, style/weight, size, live preview, and OK/Cancel confirmation.

## Complete Stable Package

- `StrataShell-Setup-1.0.18-win-x64.exe`: Full self-contained Setup and uninstaller with bundled .NET 10 runtime.
- `StrataShell-1.0.18.zip`: Complete self-contained updater and portable bundle with bundled .NET 10 runtime.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Existing installations preserve personal configurations; fresh installations seed the approved Balanced Floating layout.