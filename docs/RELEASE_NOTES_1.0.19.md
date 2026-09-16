<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.19

Stable release introducing STRATA Sphere spatial window mode, native Win32 window ownership hierarchy and z-order locking, YouTube widget volume and mute controls, Calendar editor enhancements, and accumulated window management and keybinding policy stabilization.

## STRATA Sphere Window Mode

- **Per-Monitor Spatial Canvas**: Introduced STRATA Sphere (`Super + Ctrl + S`), a 3D/canvas spatial arrangement mode operating independently per display alongside Center Stage and Desktop Mode.
- **Tabbed Lifecycle & Spatial Navigation**: Multi-tab management with add, rename, navigate, spatial drag-and-drop placement, and cross-monitor tab transfers.
- **Native Window Z-Order Locking**: Established native Win32 window ownership hierarchies to lock pane z-ordering, ensuring native app windows, quick panels, launcher surfaces, and top rail interactions remain reliably layered above canvas surfaces without flicker or unwanted canvas raising on mouse activation.
- **Integrated Live Browser & Widgets**: Direct embedding of live browser tabs with customizable page transparency, embedded desktop widgets, and game embedding support.

## YouTube & Media Widget Controls

- **Integrated Volume & Mute Controls**: Direct volume slider and mute/unmute buttons embedded into the YouTube widget card, styled in STRATA material glass aesthetics.
- **Desktop Dimming Management**: Suppressed desktop dimming during interactions in Sphered mode and synchronized volume controls with system media transport.
- **AI Command Improvements**: Expanded prompt input height for multi-line queries with live-theme reactive status indicators.

## Calendar & First-Party Polish

- **Event Editor & Date Picker**: Polished Calendar event editor, date picker controls, and refreshed dynamic calendar icons.
- **Theme Propagation**: Accent highlights, selection states, and agenda details update smoothly across live Light and Dark theme changes.

## Window Management & Keybinding Policies

- **Policy Validation**: Updated keybinding policies and validation in `BuildBindings()` ensuring all shortcut remaps remain conflict-free and reject occupied combinations and physical aliases.
- **Focus & Lifecycle Reconciliation**: Reliable focus restoration upon window closure, secondary display lifecycle reconciliation, and cross-monitor widget drag handling.

## Complete Stable Package

- `StrataShell-Setup-1.0.19-win-x64.exe`: Full self-contained Setup and uninstaller with bundled .NET 10 runtime.
- `StrataShell-1.0.19.zip`: Complete self-contained updater and portable bundle with bundled .NET 10 runtime.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Existing installations preserve personal configurations; fresh installations seed the approved Balanced Floating layout.
