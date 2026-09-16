<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.16

Stable release delivering first-party app usability revisions (Paint, Snip, and Text), tiled window interaction protections, decorative border resilience during installer runs, and launcher discovery performance.

## Paint, Snip & Text Usability

- **Quick Swatches & Color Picker**: Adds 8 quick swatches and an interactive custom color popup with visual HSV selection across STRATA Paint and STRATA Snip.
- **Paint Selection & Clipboard**: Adds rectangular selection with interactive move and resize dragging, corner handle, Copy/Cut/Paste/Delete actions, keyboard shortcuts, and seamless clipboard image pasting with prior-canvas background restoration.
- **Text Typography Panel**: Replaces menu dropdowns with a dedicated Notepad-style font configuration dialog supporting font family, weight/style, size, live sample preview, and OK/Cancel confirmation.

## Window Management & Tiling Invariants

- **Tiled Drag Protection**: Preserves Center Stage placement by ignoring unmodified title-bar drags on tiled windows. Tiled windows require explicit `Super + Ctrl` dragging to float or resize, preventing accidental layout disruption.
- **Managed WPF Drag Bounds**: Synchronizes WPF DIP dimensions with DPI scale during interactive floating drag-resizing.

## Reliability & Performance

- **Installer Popup Resilience**: Hardens active window border overlay creation against transient installer popups and Win32 error 5 (access denied), gracefully expiring decorative borders without triggering recovery UI.
- **Wallpaper Picker Selection**: Resolves single-selection file picker handling during drag interactions so multi-item clearing never triggers recovery.
- **Themed Terminal Scrollbar**: Terminal scrollbar colors derive directly from the active palette and update across live theme changes.
- **Files Scaling & App Discovery**: Scales STRATA Files footer elements at compact dimensions and runs application discovery concurrently in the background for faster launcher responsiveness.

## Complete Stable Package

- `StrataShell-Setup-1.0.16-win-x64.exe`: Full self-contained Setup and uninstaller with bundled .NET 10 runtime.
- `StrataShell-1.0.16.zip`: Complete self-contained updater and portable bundle with bundled .NET 10 runtime.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Existing installations preserve personal configurations; fresh installations seed the approved Balanced Floating layout.