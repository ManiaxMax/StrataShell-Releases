# Installation and recovery

Strata can run as a safe preview or as the current account's Windows shell. Preview never changes sign-in. Installed mode always uses an immutable local, self-contained release plus a watchdog and an external Explorer recovery path.

## Windows edition support

The installer detects the Windows edition and chooses the activation route automatically:

| Windows 11 edition | Strata activation route | Elevation |
|---|---|---|
| Home | Reversible current-user custom-shell policy | Not normally required |
| Pro | Reversible current-user custom-shell policy | Not normally required |
| Enterprise / Education / IoT Enterprise | Windows Shell Launcher for the current SID; Explorer remains its default fallback | Required when configuring Shell Launcher |

Microsoft documents Shell Launcher only for Enterprise, Education, and IoT Enterprise. Strata therefore does not attempt to enable that unavailable feature on Home or Pro. Both routes start the same guarded `StrataShell.exe --bootstrap` command and use the same recovery marker, backup, and three-failure watchdog.

The installer rejects UNC executables and never points sign-in at a mutable development build.

## What the installer configures

Installation is an orchestration and validation pass, not a file copy:

1. Detects Windows edition and selects the supported activation route.
2. Publishes or copies a self-contained x64 payload into `%LOCALAPPDATA%\Programs\StrataShell\releases\<version>`.
3. Creates the managed Light/Dark wallpaper library. A development checkout keeps `C:\Projects\StrataShell\wallpapers`; a packaged install uses `%LOCALAPPDATA%\Programs\StrataShell\wallpapers`.
4. Imports an optional `portable-profile\startup-apps.json`. Existing startup choices are preserved, missing applications are skipped, and moved executables are rebased through registered App Paths when possible.
5. Runs Strata's noninteractive self-test and stops before changing the shell if any required check fails.
6. Captures the existing per-user shell value exactly once.
7. Installs recovery scripts and Return to Explorer shortcuts outside the versioned payload.
8. Activates Strata only after the executable, self-test, backup, and recovery route verify successfully.

At first run, Strata discovers monitors, resolution, scale, orientation, panoramic layouts, battery presence, audio endpoints, network adapters, Bluetooth radios/devices, installed Win32 shortcuts, registered executables, packaged apps, startup registrations, and tray publishers. These are runtime discoveries so the same install adapts when hardware or applications change later.

Strata also verifies its idle policy at startup. The default visual screensaver begins after five minutes. Existing Windows display/sleep/screen-saver values of Never or 15 minutes and longer are preserved; shorter nonzero values are raised to 15 minutes so Windows does not blank, sleep, or lock before the Strata surface appears. An administrator-enforced inactivity policy below 15 minutes is reported in Settings rather than silently overridden.

## Safe preview

```powershell
git clone https://github.com/ManiaxMax/StrataShell.git
cd StrataShell
dotnet restore .\src\StrataShell\StrataShell.csproj
dotnet run --project .\src\StrataShell\StrataShell.csproj -- --preview
```

Before activation, verify the rail, widgets, launcher, Settings, Files, app launching, workspaces, movement, floating, fullscreen, ChatGPT/Codex, and `Ctrl + Alt + Shift + Delete` recovery chord.

## Source-tree installation

Build and install without changing the configured shell:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\scripts\Install-StrataShell.ps1
```

Install, validate, and activate using edition detection:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\scripts\Install-StrataShell.ps1 -Activate -ActivationMode Auto
```

`scripts\Install-StrataShell.cmd` is the equivalent one-click entry. Sign out and back in after successful activation. A Shell Launcher feature enablement can require a restart.

`-ActivationMode Policy` explicitly selects the per-user route. `-ActivationMode ShellLauncher` is accepted only when the detected edition supports it; forcing it on Home or Pro fails before any shell change.

## Self-contained installer bundle

Build a redistributable bundle:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\scripts\Build-InstallerBundle.ps1
```

The output is written beneath `artifacts\installer` as a folder and ZIP. It contains a self-contained payload, installer/recovery scripts, empty managed Light/Dark wallpaper folders, and a root **Install StrataShell.cmd** entry. The target PC does not need the .NET SDK.

To carry startup choices into the bundle:

```powershell
.\scripts\Build-InstallerBundle.ps1 -PortableProfile "$env:USERPROFILE\Documents\Strata"
```

Personal wallpaper libraries are not automatically embedded because they can be very large. Add supported images to the bundle's `wallpapers\WallpapersLight` and `wallpapers\WallpapersDark` folders before distribution, or copy them into the installed managed folders later. Strata's live watcher makes additions immediately available to the chooser and wallpaper shortcuts.

## Updating

Settings → Updates is the normal installed route:

1. Choose Stable or Preview.
2. Connect GitHub once for the current Windows account. Repository owners use a read-only fine-grained token; accepted collaborators currently use their own classic token with repository access.
3. Select **Check for updates**, review the discovered release, and select **Update now**.
4. Strata downloads the private release asset, enforces archive bounds and paths, verifies GitHub's SHA-256 digest when present, runs the normal payload self-test, and installs an immutable release directory.

The credential is stored in Windows Credential Manager and never in Strata's JSON, logs, or installer command line. Removing a user's repository access prevents that user from retrieving later private releases. The currently running shell is never overwritten or interrupted; an activated installation uses the new release at the next sign-in.

Maintainers publish assets through `.github/workflows/release.yml`. A stable release is an ordinary GitHub Release; a preview is marked prerelease. Both channels require an installer asset named `StrataShell-<version>.zip`, which the workflow creates only after the bundled quiet self-test passes.

For a source-tree update, run the activation command again. A new timestamped release is published and self-tested before the current account's shell target changes.

`-ExistingRelease` accepts either an already installed release or an external self-contained payload. External payloads are copied into a new managed release directory before validation and activation.

## Portable startup profile

Settings → Startup + Background Apps can export `%USERPROFILE%\Documents\Strata\startup-apps.json`. On a new PC, supply that file with `-PortableProfile`, place it beside the packaged installer as `portable-profile\startup-apps.json`, or copy it to the same Documents path.

Strata reconciles each manifest hash once. It never overwrites an existing same-name startup choice during automatic import, rejects command interpreters and network paths, and skips applications that are not installed on the new PC. The launcher itself is rebuilt from the target PC each time, with broken shortcuts/helper entries removed and administrative tools given friendly names.

## Restore Explorer

The Power panel's **Explorer Session** action is intentionally temporary: it closes Strata, starts `explorer.exe`, and leaves the default-shell marker and Windows shell registration unchanged. After sign-out or restart, the configured login route starts Strata again.

For a permanent change, use **Settings → Windows Tweaks → Default Shell** and turn off **Make Strata My Default Shell**. That confirmed action uses the Windows-edition-appropriate restore route. The emergency methods below are also permanent recovery actions rather than temporary Explorer sessions.

From Strata, press:

```text
Ctrl + Alt + Shift + Delete
```

From Task Manager, a terminal, or the desktop recovery shortcut, run:

```text
%USERPROFILE%\Strata Recovery\Return-To-Explorer.cmd
```

Recovery removes Strata's marker, restores the captured per-user shell value, removes legacy Strata values, signals the bootstrap to stop, and starts `explorer.exe` immediately. It invokes the elevated Shell Launcher cleanup only when the installation record says that route was used; Home/Pro recovery does not call the unavailable IoT helper.

If the UI is unavailable, open Task Manager with `Ctrl + Shift + Esc`, choose **Run new task**, and run the recovery command above. Windows Recovery, sign-in, UAC secure desktop, and `Ctrl + Alt + Delete` remain Windows-owned.

## Bootstrap watchdog

The configured login command is `StrataShell.exe --bootstrap`. Bootstrap launches the real `--shell` process and watches it. Three exits inside 45 seconds remove the active marker and start Explorer instead of continuing a crash loop.

## State and diagnostics

Installed and recovery state is stored under `%LOCALAPPDATA%\StrataShell`. Important files include:

- `Recovery\installation.json`
- `Recovery\shell-backup.json`
- `Recovery\shell-launcher.json` on Shell Launcher editions
- `Recovery\self-test.json`
- `startup-import.sha256`
- `strata.log`

These files can contain local paths, account identifiers, application names, and machine-specific state and should not be published.
