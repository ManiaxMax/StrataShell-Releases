# Installation and recovery

STRATA Shell can run as a safe preview or as the current account's Windows shell. Preview never changes sign-in. Installed mode always uses an immutable local release plus a watchdog and an external Explorer recovery path. Stable packages are self-contained; lightweight owner-testing updates on the Preview channel are framework-dependent.

## Windows edition support

The installer detects the Windows edition and chooses the activation route automatically:

| Windows 11 edition | STRATA Shell activation route | Elevation |
|---|---|---|
| Home | Reversible current-user custom-shell policy | Not normally required |
| Pro | Reversible current-user custom-shell policy | Not normally required |
| Enterprise / Education / IoT Enterprise | Windows Shell Launcher for the current SID; Explorer remains its default fallback | Required when configuring Shell Launcher |

Microsoft documents Shell Launcher only for Enterprise, Education, and IoT Enterprise. STRATA Shell therefore does not attempt to enable that unavailable feature on Home or Pro. Both routes start the same guarded `StrataShell.exe --bootstrap` command and use the same recovery marker, backup, and three-failure watchdog.

The installer rejects UNC executables and never points sign-in at a mutable development build.

## What the installer configures

Installation is an orchestration and validation pass, not a file copy:

1. Detects Windows edition and selects the supported activation route.
2. Publishes or copies an x64 payload into an immutable directory beneath `%LOCALAPPDATA%\Programs\StrataShell\releases` (self-contained for Stable, framework-dependent for the owner Preview test channel). If the canonical version directory already contains different or locked files, the installer preserves it and selects a deterministic content-fingerprinted directory instead of deleting it.
3. Creates the managed Light/Dark wallpaper library. A development checkout keeps `C:\Projects\StrataShell\wallpapers`; a packaged install uses `%LOCALAPPDATA%\Programs\StrataShell\wallpapers`.
4. Imports an optional `portable-profile\startup-apps.json`. Existing startup choices are preserved, missing applications are skipped, and moved executables are rebased through registered App Paths when possible.
5. Runs STRATA Shell's noninteractive self-test and stops before changing the shell if any required check fails.
6. Captures the existing per-user shell value exactly once.
7. Installs recovery scripts and Return to Explorer shortcuts outside the versioned payload.
8. Activates STRATA Shell only after the executable, self-test, backup, and recovery route verify successfully.

For a brand-new Phase 5 release installation, Setup and the portable installer seed Balanced quality, Floating mode and the [approved widget layout](SHIPPED_DEFAULTS.md), with Clock and YouTube locked expanded. Seeding happens only if no settings file exists. Updates, repairs and ordinary shell startup do not overwrite existing preferences. Explicit Reset Settings is separate.

At first run, STRATA Shell discovers monitors, resolution, scale, orientation, panoramic layouts, battery presence, audio endpoints, network adapters, Bluetooth radios/devices, installed Win32 shortcuts, registered executables, packaged apps, startup registrations, and tray publishers. These are runtime discoveries so the same install adapts when hardware or applications change later.

STRATA's visual screensaver begins after the configured idle interval. Windows display-off, sleep and managed-lock settings are preserved; startup does not silently raise them. The native Power + Session page changes supported active-plan timeouts only after an explicit user action. Organization policy remains authoritative.

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

The output is written beneath `artifacts\installer` as a folder and ZIP. It contains a self-contained payload, installer/recovery scripts, exactly the two approved Light/Dark wallpapers, and a root **Install STRATA Shell.cmd** entry. The target PC does not need the .NET SDK.

The self-contained desktop runtime includes Windows Forms and Drawing components required by the notification area. Shell and Launcher use the same runtime selection; package checks reject missing tray dependencies before installation.

To carry startup choices into the bundle:

```powershell
.\scripts\Build-InstallerBundle.ps1 -PortableProfile "$env:USERPROFILE\Documents\Strata"
```

Personal wallpaper libraries are never embedded in an official package. Users may add supported images to their installed managed folders afterward; STRATA Shell's live watcher makes local additions immediately available to the chooser and wallpaper shortcuts.

## Updating

Settings → Updates is the normal installed route:

1. Choose Stable or Preview.
2. Select **Check for updates**, review the discovered release, and select **Update now**.
3. STRATA Shell downloads the public binary-only release asset, enforces archive bounds and paths, requires GitHub's SHA-256 digest and a release-bound manifest signed by the trusted STRATA update key, runs the normal payload self-test, and installs an immutable release directory.

The currently running shell is never overwritten or interrupted; an activated installation uses the new release at the next sign-in. Source remains private in `ManiaxMax/StrataShell`, while public update assets are published only to `ManiaxMax/StrataShell-Releases`.

The install gate validates STRATA itself, not the presence of optional third-party applications. Optional integrations such as ChatGPT are reported as available or unavailable without preventing installation. If a required self-test does fail, the installer includes the failed check name and detail in its error output as well as preserving the complete JSON report.

In active shell mode, the updater treats the release directory containing the running shell executable as authoritative when recovery metadata is stale and atomically reconciles that record without overriding a valid newer release waiting for sign-in. Merely opening another installed release in Preview mode does not change installation metadata. Every download uses a unique staging attempt, so a stale extraction cannot block a retry. A retry of the same verified tag never deletes the active executable: an identical payload reuses its immutable directory, while different or partial locked content is preserved and the verified payload installs into a deterministic collision-safe directory. The canonical release marker keeps update comparisons tied to the GitHub tag rather than the physical fallback directory name.

The default owner test package is a prerelease targeting the distribution repository's `preview` branch. It is framework-dependent and intentionally omits the .NET runtime, full Setup executable, wallpaper library, and Stable-only extras. A machine with a registered .NET 9 desktop runtime uses it normally. When upgrading from a compatible self-contained Stable release, the installer can instead hard-link the exact hash-verified runtime files already present in that Stable installation; the public Preview ZIP still contains no runtime binaries. After approval, promotion to `main` builds the complete self-contained Stable installer and bundle.

Update failures remain in the Updates status surface, are written to `%LOCALAPPDATA%\StrataShell\Recovery\last-update-error.txt`, and use a foreground STRATA dialog with **Copy Error** so the complete diagnostic can be copied before closing it.

For a source-tree update, run the activation command again. A new timestamped release is published and self-tested before the current account's shell target changes.

`-ExistingRelease` accepts either an already installed release or an external self-contained payload. External payloads are copied into a new managed release directory before validation and activation.

## Portable startup profile

Settings → Startup + App Tray can export `%USERPROFILE%\Documents\Strata\startup-apps.json`. On a new PC, supply that file with `-PortableProfile`, place it beside the packaged installer as `portable-profile\startup-apps.json`, or copy it to the same Documents path.

STRATA Shell reconciles each manifest hash once. It never overwrites an existing same-name startup choice during automatic import, rejects command interpreters and network paths, and skips applications that are not installed on the new PC. The launcher itself is rebuilt from the target PC each time, with broken shortcuts/helper entries removed and administrative tools given friendly names.

## Restore Explorer

The Power panel's **Explorer Session** action is intentionally temporary: it closes STRATA Shell, starts `explorer.exe`, and leaves the default-shell marker and Windows shell registration unchanged. After sign-out or restart, the configured login route starts STRATA Shell again.

For a permanent change, use **Settings → Windows Tweaks → Default Shell** and turn off **Make STRATA Shell My Default Shell**. That confirmed action uses the Windows-edition-appropriate restore route. The emergency methods below are also permanent recovery actions rather than temporary Explorer sessions.

From STRATA Shell, press:

```text
Ctrl + Alt + Shift + Delete
```

From Task Manager, a terminal, or the desktop recovery shortcut, run:

```text
%USERPROFILE%\Strata Recovery\Return-To-Explorer.cmd
```

Recovery removes STRATA Shell's marker, restores the captured per-user shell value, removes legacy STRATA values, signals the bootstrap to stop, and starts `explorer.exe` immediately. It invokes the elevated Shell Launcher cleanup only when the installation record says that route was used; Home/Pro recovery does not call the unavailable IoT helper.

If the UI is unavailable, open Task Manager with `Ctrl + Shift + Esc`, choose **Run new task**, and run the recovery command above. Windows Recovery, sign-in, UAC secure desktop, and `Ctrl + Alt + Delete` remain Windows-owned.

## Bootstrap watchdog

The configured login command is `StrataShell.exe --bootstrap`. Bootstrap launches the real `--shell` process and watches it. Three unexpected exits within ten minutes start a temporary Explorer session while preserving the active marker and selected next-login shell; zero exits count, while deliberate restart and Explorer requests do not. Missing-payload and launch-failure fallback are temporary too. Bootstrap also offers Keep Waiting or a confirmed temporary Explorer session when the shell dispatcher stalls. The warning runs outside the shell process, defers around startup/resume/lock, and never automatically kills a slow shell. Unsaved work in STRATA apps can be lost if the user chooses to close it; external apps are left running.

## State and diagnostics

Installed and recovery state is stored under `%LOCALAPPDATA%\StrataShell`. Important files include:

- `Recovery\installation.json`
- `Recovery\shell-backup.json`
- `Recovery\shell-launcher.json` on Shell Launcher editions
- `Recovery\self-test.json`
- `startup-import.sha256`
- `strata.log`

These files can contain local paths, account identifiers, application names, and machine-specific state and should not be published.

## Signed update transition

Batch 1 embeds the verification key; testers need no signing setup. Existing 1.0.11/1.0.12 archive and installer-discovery checks accept the signed layout. Hardened clients reject unsigned automatic updates. Lightweight Preview reuses the Stable runtime; fresh users should install full Stable first. Windows publisher signing remains separate and is not configured yet. See [release signing and recovery](RELEASE_SIGNING.md).
