# Strata Shell 1.0.0

This installer packages the same Strata application build that was running when the release was prepared:

- source release: `20260828-floating-refresh-04`
- product version: `1.0.0+0c1df5722d4fd2f87bc528437706879a4d105f21`
- `StrataShell.exe` SHA-256: `E4129F31D09BFABBF23FD3059AB4C6BE7D7E011C64C0D07F6101615A8E35C69D`
- `StrataShell.dll` SHA-256: `D779CA3317244E8812C424BBBD98F8C5E4E26A4BB9A15BFF8EB79A85BF3162FB`

The executable and application code are taken from that active release. Generated WebView2 profile data and PDB files are excluded, and the non-executable local PDB path in the copied application assembly is reduced to the portable filename before packaging.

## Installer and launcher

- Self-contained win-x64 package with the .NET 9 desktop runtime included.
- Polished per-user setup with finish options to set Strata as the default shell and launch it immediately.
- Strata Launcher in Start Menu Programs for preview launch, shell activation/restoration, configuration reapplication, update, repair, and safe settings reset.
- Start Menu recovery and uninstall entries plus a desktop `Return to Explorer` shortcut.
- Windows Add/Remove Programs registration and a dedicated uninstaller.

## Wallpapers and privacy

- Includes only the 20 `Strata_*` PNG files from `WallpapersLight` and the 20 `Strata_*` PNG files from `WallpapersDark`.
- Does not package personal wallpapers, recent-wallpaper history, the current weather location, credentials, sessions, search history, account data, logs, caches, recovery records, or browser-profile data.
- Fresh installs use the documented schema-22 allowlist in `installer/Defaults/settings.safe.json`; existing settings are retained during install/update.
- The portable default pair is `Strata_Green_Black_Abstract_4K.png` and `Strata_Green_White_Abstract_4K.png`.

## Recovery and uninstall

Before Strata can change persistent Windows state, setup records the original per-user shell policy, legacy shell value, Windows light/dark theme values, screen-saver timeout, active power-plan display/sleep timeouts, and lock-screen image. Uninstall stops Strata before restoration, returns Explorer as the shell, restores those captured values, re-enables startup applications disabled through Strata, removes its shortcuts and Add/Remove Programs entry, and asks whether to retain settings and wallpapers.

Legacy Windows Shell Launcher activation is restored through the existing administrator-approved recovery helper when that older activation mode is detected. Explorer is started at the end even if an uninstall step reports a recoverable error.

## Notes

- This build is per-user and currently unsigned; Windows may show its standard unrecognized-publisher warning.
- Online update and release access use the private `ManiaxMax/StrataShell` GitHub repository and require an account/token with suitable repository permissions.
