# STRATA Shell 1.0.6

Stable packaging and release-identity correction for STRATA Shell. This patch makes the Shell, Launcher, installer metadata, shortcuts, and default-shell target one versioned release unit.

## Corrected Stable installation

- Packages a version-matched STRATA Launcher and `StrataMaintenance` with every installable Stable and Preview payload.
- Includes the safe defaults and reviewed shell-policy helper required by Launcher repair and default-shell actions.
- Refuses to build, publish, download, or install an incomplete release archive.
- Uses one backward-compatible installation record for Setup, portable installs, in-app updates, Launcher actions, and recovery.
- Records matching Shell, Launcher, install directory, activation state, and uninstaller paths.
- Ensures a Launcher opened from Stable 1.0.6 configures Stable 1.0.6 as the next Windows shell, never an older release.

## Release identity

- Reports Running Build, Installed Release, and Engine as separate values.
- Advances Engine to 1.0.6 only with this Stable promotion; timestamped Preview builds do not advance Engine.
- Preserves a pending installed Stable release while an older Preview session remains active until sign-out.

## Standardized release gates

- Validates the matching Shell and Launcher product versions, required component inventory, runtime policy, safe defaults, policy helper, and canonical install record.
- Publishes both the full Setup and portable ZIP with recorded SHA-256 values.
- Independently downloads and verifies both public Stable packages before publication is considered complete.
- Keeps Preview lightweight and runtime-free while Stable remains self-contained with the required .NET 9 runtime.

## Stable package

- `StrataShell-Setup-1.0.6-win-x64.exe`: full self-contained Setup and uninstaller.
- `StrataShell-1.0.6.zip`: complete self-contained updater/portable bundle.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Exactly the two approved library wallpapers, `STRATA_LOGO_LIGHT.jpg` and `STRATA_LOGO_DARK.jpg`, plus the internal original STRATA fallback.
