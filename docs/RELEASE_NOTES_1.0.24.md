# STRATA Shell 1.0.24

This Stable release promotes the validated Preview work since 1.0.23.

## Desktop and dock

- Dock Mode now offers the pinned/running app shelf at the top in Tiled mode and at the bottom in Floating mode. Icons enlarge toward the desktop without moving resting buttons; previews and status panels clear the dock envelope. Tray arrows follow the panel-opening direction.
- Super + W cycles through Center Stage, the two wide views, and separate Dynamic / Dwindle, Grid, Columns, and Main window and stack layouts. Existing per-monitor preferences and saved setups are preserved.
- Native STRATA Floating apps retain their outgoing workspace animation. Secondary rail rendering avoids persistent text/icon bitmap caches, and work-area ownership handles inherited desktop reservations more carefully.

## Reliability and first-party apps

- Launcher and Sphere application catalogs refresh asynchronously, including manual refresh and discovery changes.
- STRATA Files supports larger explicit archive extraction, while preview-cache limits, path validation, cancellation and preservation of existing files remain enforced. Disabled widgets no longer displace enabled cards.
- Terminal script quoting, encrypted draft quotas, thumbnail-provider containment and detached desktop resource cleanup are strengthened.
- Settings offers allowlisted local diagnostic exports, an optional short performance recording, and a read-only recovery rehearsal. File-operation history supports guarded manual retry; it does not silently resume operations.
- All 25 interface catalogs contain 3,897 messages, including the new layouts and dock controls. Translation generation is local; native-speaker editorial review remains open.

## Release assets and limits

- Setup and the portable installer require explicit acceptance of the STRATA end-user terms and desktop-shell risk notice before installation. The terms include warranty disclaimers and liability limits while preserving non-waivable rights.
- `StrataShell-Setup-1.0.24-win-x64.exe`: self-contained Setup and uninstaller.
- `StrataShell-1.0.24.zip`: self-contained updater and portable bundle.
- Both include the .NET 10 Desktop runtime and exactly 24 approved 4K wallpapers. Checksums, a manifest and isolated installer validation accompany the release. Existing user settings are preserved.

STRATA remains experimental alpha software. Signed update archives are mandatory; Windows Authenticode publisher signing is not configured. Physical multi-monitor input/DPI, installed visual behavior, native screen-reader behavior and hardware/GPU paths still require acceptance. Source and isolated tests are not installed acceptance. Publication does not install, activate or restart STRATA Shell.
