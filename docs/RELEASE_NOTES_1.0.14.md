<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.14

Stable release delivering browser workspace reliability fixes, native browser focus handling, installer profile preservation during updates, and repository cleanup.

## Browser Workspace Reliability

- **Native Focus Preservation**: Prevents WebView2 native window handles from deadlocking or losing focus during workspace moves.
- **Teardown Safety**: Ensures browser native content and child HWNDs are cleanly released before window teardown, preventing shell hangs during multi-window navigation.
- **Stall Tracing**: Diagnoses and guards against transition stalls when navigating across Center Stage workspaces with embedded browsers.

## Installer & Update Preservations

- **Profile Routing Fix**: Portable and Setup installers reliably route user preferences to `%LOCALAPPDATA%\StrataShell\settings.json`, preventing existing installations without bundled wallpapers from being erroneously treated as fresh installs.
- **Update Safety**: Seamless updates between Stable and Preview channels without configuration overwrites.

## Complete Stable Package

- `StrataShell-Setup-1.0.14-win-x64.exe`: Full self-contained Setup and uninstaller with bundled .NET 10 runtime.
- `StrataShell-1.0.14.zip`: Complete self-contained updater and portable bundle with bundled .NET 10 runtime.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Existing installations preserve personal configurations; fresh installations seed the approved Balanced Floating layout.