# STRATA Shell 1.0.1

Official release package for STRATA Shell 1.0.1, featuring a comprehensive first-party application suite, simplified update channels, and fully self-contained .NET 9 desktop runtime packaging.

## What's New in 1.0.1

### First-Party Application Suite
- **STRATA Paint**: Purpose-built canvas drawing, markup, image editing, and palette export with live STRATA theme matching.
- **STRATA Video Player**: Hardware-accelerated media playback designed to integrate seamlessly into STRATA Shell's window management.
- **STRATA Photo / Image Viewer**: High-performance photo inspection and gallery navigation with zoom, metadata inspection, and instant wallpaper setting.
- **STRATA Terminal, Text Editor & Snip**: Streamlined first-party productivity tools with unified glass materials and theme synchronization.

### Simplified Update Experience
- **Unauthenticated Public Updates**: Removed all developer PAT token prompts and complex credential configuration.
- **Channel Selector**: Easily toggle between **Stable** and **Preview** channels in `Settings > Updates`.
- **In-Place Updates**: Check for updates and install the latest packages directly inside STRATA Shell with real-time download and installation progress.
- **Pinned Launcher Updates**: STRATA Launcher defaults to the latest verified Stable release for safety.

### Self-Contained Runtime & Standalone Installer
- **Bundled .NET 9 Desktop Runtime**: All binaries include the complete self-contained .NET 9 runtime (`coreclr.dll`, `hostfxr.dll`). No external .NET installation or runtime prompts required.
- **Single-File Setup**: `StrataShell-Setup-1.0.1-win-x64.exe` installs and registers STRATA Shell with automatic edition-aware activation and safe Explorer recovery.
- **Portable Bundle**: `StrataShell-1.0.1.zip` provides a complete standalone directory release.

### Approved STRATA wallpaper library
- Includes only `STRATA_LOGO_LIGHT.jpg` and `STRATA_LOGO_DARK.jpg`; the original `strata-signal.png` remains an emergency fallback rather than a selectable library entry.

## Checksums & Artifacts

- **Installer**: `StrataShell-Setup-1.0.1-win-x64.exe` (SHA-256: `17D0C858BD9558483AD7ACF436AE0C029D5C45499599CABA5971CF5E881E595D`)
- **Bundle**: `StrataShell-1.0.1.zip` (SHA-256: `DE72F35B828E05B9E8D937BA3CE3C0E2EEC5AEBBE6A32549610C4DF91451B253`)
- **Validation**: Detailed acceptance and self-test verification data available in `VALIDATION_REPORT.json` and `RELEASE_MANIFEST.json`.
