<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.png" alt="STRATA Shell" width="600" />
</p>

# STRATA Shell 1.0.23

This Stable release promotes the Preview work since 1.0.22: an initial multilingual interface, a Windows permissions route in STRATA Files, and clearer shutdown-recovery reporting.

## Interface languages

- Settings > Language offers 25 language/region choices and Follow Windows. Shared labels, Settings navigation and search update without restarting STRATA Shell.
- Arabic, Hebrew, Persian and Urdu use right-to-left layouts in Settings and shared dialogs. Untranslated text remains in English; many app-specific screens, errors, Setup and Launcher are not yet translated. Native-speaker review remains open.
- The preference is portable and preserves existing English behavior on upgrade.

## Files and recovery

- Physical files and folders offer a Permissions action that opens the Windows Security properties page. Windows owns permission editing and any required elevation; virtual archive and Recycle Bin entries do not offer the action.
- Failed shutdown save or cleanup steps are recorded locally by category, and STRATA warns on the next start. A workspace-transition code move preserves the existing behavior while separating its owner.
- Settings and release-status text more clearly distinguish source, packaged, installed and running builds, and disclose that Windows Authenticode publisher signing is not configured.

## Release assets

- `StrataShell-Setup-1.0.23-win-x64.exe`: self-contained Setup and uninstaller.
- `StrataShell-1.0.23.zip`: self-contained updater and portable bundle.
- Both include the .NET 10 Desktop runtime and the 24 approved 4K wallpapers. Checksums, a manifest and an isolated installer validation report accompany the packages.

Source and isolated checks do not replace physical installed-shell acceptance. Publication does not install, activate or restart STRATA Shell.
