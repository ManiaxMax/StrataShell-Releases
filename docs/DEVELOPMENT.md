# Development and testing

## Toolchain

- Windows 11 x64
- .NET 9 SDK
- WPF targeting `net9.0-windows10.0.26100.0`
- x64 runtime identifier
- Microsoft Edge WebView2 Runtime

NuGet dependencies are declared in `src/StrataShell/StrataShell.csproj`:

- `Microsoft.Web.WebView2`
- `NAudio.Wasapi`
- `ManagedShell`

## Restore and build

```powershell
dotnet restore .\src\StrataShell\StrataShell.csproj
dotnet build .\src\StrataShell\StrataShell.csproj -c Release
```

The project treats compiler warnings as errors. Generated `bin` and `obj` trees are ignored by Git.

For targeted floating-mode acceptance, run `scripts/Test-StrataDesktopMode.ps1` after a Release build. It creates a private, noninteractive Windows desktop and an isolated settings directory, runs real WPF/Win32 probes, writes JSON plus Light/Dark dock captures under `artifacts/desktop-mode-acceptance`, and disposes that desktop. It never switches the input desktop or registers a native appbar. The internal `--desktop-mode-test` entry point refuses the normal interactive desktop. Native appbar negotiation, physical hotkeys, third-party apps, and installed multi-monitor behavior still require coordinated live acceptance. Quiet self-tests read migrated settings without persisting profile migrations.

## Run modes

| Arguments | Purpose |
|---|---|
| none or `--preview` | Safe interactive preview; does not change the configured Windows shell |
| `--shell` | Full shell behavior used by the installed bootstrap |
| `--no-desktop` | Start the coordinator without the desktop surface |
| `--bootstrap` | Recovery watchdog that owns the installed shell child process |
| `--self-test --quiet` | Noninteractive acceptance suite with process exit status |
| `--restore-explorer` | Request Explorer restoration |
| `--activate-chatgpt` | Direct packaged-app activation path |
| `--critical-ui-test` | Live first-party surface acceptance route |
| `--audio-loopback-test` | Live WASAPI loopback acceptance route |

Do not run `--shell`, `--critical-ui-test`, or UI automation over somebody's active shell session without coordinating the test. They can take over desktop surfaces or focus.

## Self-test

Build first, then run:

```powershell
.\src\StrataShell\bin\Release\net9.0-windows10.0.26100.0\win-x64\StrataShell.exe --self-test --quiet
```

The suite reports its live check count in `%LOCALAPPDATA%\StrataShell\Recovery\self-test.json`; do not hard-code an older count. Coverage includes:

- required install and recovery paths;
- settings parsing and persistence;
- wallpaper libraries and fallback assets;
- packaged ChatGPT discovery;
- keybinding uniqueness, protected routes, remap/reset/conflict policy, and viewer/editor XAML construction;
- one/two-app Center Stage geometry;
- widget-column and vertical-envelope calculations;
- frame compensation and chrome policy;
- workspace overflow and new-window placement;
- mixed native/STRATA composition timing and animation-time material suspension;
- wallpaper/frost presentation handoff and palette-wide chromatic bloom;
- launcher keyboard selection;
- forced new browser-window launch behavior; and
- recovery configuration consistency.

The report is written to `%LOCALAPPDATA%\StrataShell\Recovery\self-test.json`. A test that depends on local wallpaper folders or installed applications may fail on a clean development machine; document the machine dependency instead of weakening the installed-shell gate.

## Visual acceptance checklist

After changing layout, animation, DWM, theme, or XAML behavior, inspect a real preview at the target DPI and refresh rate:

1. Rail reserves space and does not overlap centered applications.
2. One app aligns to the rendered top and bottom widget-card edges.
3. Two apps share equal visible width and fit the same vertical envelope.
4. UI Scale keeps widgets and Center Stage aligned.
5. Active border follows the wallpaper palette.
6. Workspace slides travel completely off-screen without a midpoint disappearance.
7. Top/bottom swaps animate cleanly and land on exact geometry.
8. Fullscreen becomes opaque and the rail, dock, launcher, and quick panels stay suppressed at monitor edges.
9. Launcher/Settings/Files receive keyboard focus immediately.
10. Light and dark wallpapers update shell and Windows app-theme preference.
11. Frosted surfaces retain the outgoing wallpaper until the desktop crossfade finishes.
12. Colored text, icons, borders, graphs, and status colors bloom in their own palette color while neutral body copy stays legible.
13. `Super + Shift + D` keeps native and first-party STRATA apps visually locked together without shader/frost jitter.

## Source layout

```text
src/StrataShell/
├── App.xaml(.cs)             Process modes, startup, and shell composition
├── Interop/                  Win32, DWM, audio, and appbar boundaries
├── Models/                   Settings, palette, app, file, and keybind data
├── Services/                 Policy, orchestration, integrations, and state
├── Themes/Controls.xaml      Shared first-party control styling
└── Views/                    Desktop, rail, panels, launcher, Files, Settings
```

The private-desktop runner also checks widget input with YouTube above and below Spectrum/AI: live hit targets, rejection of stale YouTube result coordinates, editor focus transfer and text entry, and embedded-player capture/release during seek gestures. It writes `widget-input.png` alongside the report. These probes do not certify live YouTube playback or physical input in the installed shell.

Read [Architecture](ARCHITECTURE.md) before changing window ownership, focus, hotkey, recovery, or workspace policy.

## Design constraints

- Center Stage is the only current layout model. Do not reintroduce stale recursive tiling or free-floating defaults.
- One workspace has at most two managed applications.
- Widget columns are persistent desktop infrastructure, not ordinary managed app windows.
- Visible frame geometry, not raw Win32 bounds, defines equal width/alignment.
- Recovery must remain independent of the global `Super` hotkey service.
- Secure Windows enrollment and authentication surfaces must remain Windows-owned.
- Local paths, SIDs, logs, user settings, personal wallpaper libraries, and WebView2 profiles must never be committed.

## Pull-request verification

At minimum:

```powershell
dotnet restore .\src\StrataShell\StrataShell.csproj
dotnet build .\src\StrataShell\StrataShell.csproj -c Release
```

Run `--self-test --quiet` and the visual checklist when the target machine provides the required installed-environment dependencies. The ordinary GitHub build workflow performs a clean restore/build. The release workflow additionally creates a self-contained installer bundle and runs its quiet payload self-test before publishing an update asset.

Shell and Launcher explicitly reference the Windows Forms desktop framework because ManagedShell's notification host/pointer helpers depend on it. Their shared self-contained runtime must match. Payload/archive validation requires the Forms, Forms.Primitives and Drawing.Common assemblies; the notification-area self-test exercises the pointer helper so a WPF-only package cannot pass unnoticed. Validate packaged tray interactions as well as framework-dependent source builds.

## Publishing locally

Create a self-contained release without changing shell configuration:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\scripts\Install-StrataShell.ps1
```

See [Installation and recovery](INSTALLATION.md) for activation. Never point Shell Launcher at a mutable development build.

## Default owner testing procedure

1. Build the private source checkout on `preview` and run the quiet self-test.
2. Privacy-scan and commit the exact tested source, push it to `ManiaxMax/StrataShell` on `preview`, and verify the remote commit matches.
3. Run `scripts\Publish-StrataPreviewRelease.ps1`. It produces a framework-dependent binary update ZIP, targets the public distribution repository's `preview` branch, and publishes it as a prerelease.
4. Independently download the published ZIP and verify its digest and expected `payload` plus installer-script structure.
5. Test through **Settings → Updates → Preview → Check for updates → Update now**.

Preview testing deliberately excludes the bundled .NET runtime, full Setup executable, wallpaper library, and other Stable-only extras. Build those only after explicit approval to promote the tested source to `main`.

The publisher runs packaged installer and Stable-to-Preview bridge checks in child Windows PowerShell processes. Those children preserve the real user's profile/temp environment so the managed host can load, while the publisher clears inherited .NET host overrides, explicitly restores the inbox Windows PowerShell module roots when launched from a PowerShell 7/Codex host, and lets the bridge check independently disable global runtime lookup. Do not replace that targeted isolation with `Start-Process -UseNewEnvironment`; on Windows PowerShell it can substitute machine-profile values and fail before the test script starts.

### Release identity rules

- **ENGINE** is the semantic Stable baseline (`major.minor.patch`). It changes only when a tested release is deliberately promoted to Stable.
- **BUILD** is the immutable release identifier. Preview builds use their timestamp tag and therefore change on every published Preview without advancing ENGINE.
- **CHANNEL** selects which public release feed is checked; it does not alter ENGINE.
- The Preview publisher compares all project versions with the latest public Stable release and refuses to publish an accidental Engine bump. The Stable builder requires its requested release version, all project versions, and the shell assembly/file versions to match.

## Phase 4 isolated app acceptance

After a Release build, `scripts/Test-StrataAppPolish.ps1` starts `--app-polish-test` on a newly created private Windows desktop and never switches the input desktop. The application refuses this test mode outside a `StrataAppTest-` desktop. Test profiles, transfer fixtures, screenshots and reports stay under the selected output directory. A test-only `STRATA_APP_TEST_FILTER` environment variable accepts comma-separated surface names (for example `terminal,snip`) for focused regression runs.

The full suite renders all first-party app surfaces plus Files and Settings in Light/Dark at 100/150/200% raster scales, exercises shared Open/Save ownership, verifies terminal startup/output/Ctrl+C/navigation denial, Text stale-open handling, file partial-success reporting and browser permission reset. These raster checks are not a substitute for native mixed-monitor DPI, physical input, audible playback or low-power GPU tests. `--self-test --quiet` adds Phase 4 storage, native-copy cancellation, pixel-orientation, capture-budget, process-identity and real pseudoconsole tests.

App acceptance also saves native browser/terminal captures, verifies Light/Dark text contrast, exercises all compact Task Manager destinations, and records real CPU clock/disk/GPU counters in `task-hardware.json`. Its live-hardware check requires those providers on the test host; an unsupported driver can fail that acceptance even when the app correctly reports Unavailable. Private desktop regression covers Up/Down monitor selection followed by repeated workspace Left/Right/Tab commands in Tiled and Floating modes, with empty/occupied destinations and a deliberately delayed prior focus request.

The final Phase 4 evidence is linked in [current validation and limitations](STATUS.md). The desktop runner allows ninety seconds for the expanded sequence. A private comparison copy with the old focus-routing behavior restored fails those sequences; it is a diagnostic comparison, not an installable candidate. Temperature coverage on this host verifies NVIDIA readings and an honest missing CPU-provider state; it does not validate physical CPU sensors on other machines.

## Phase 5 preservation and maximize regression

After building Maintenance and Shell in Release, run `scripts/Test-StrataReleaseDefaults.ps1` with PowerShell 7. It exercises the real Setup and portable seeders with new, existing, partial and malformed profile fixtures. Existing files must remain byte-identical; the safe template is never applied on shell startup. App acceptance also reopens customized runtime profiles to detect startup overrides.

Run `scripts/Test-StrataDesktopMode.ps1` for native per-monitor work-area publication, repeated maximization, custom chrome, high-DPI-sized bar reservations, normal restore bounds and fullscreen separation. Its owned work-area probes run only on a noninteractive desktop and release their reservations in `finally`/`using` cleanup. Normal app tests still disable global theme and hardware mutations. Physical ChatGPT and other toolkit checks on the installed candidate remain required before claiming the reported maximize issue is resolved for the owner.

The full app suite includes stale deferred tray activation cancellation. Build with warnings as errors, keep reports outside Git, and use [release status](STATUS.md) to distinguish source validation from installation and visible acceptance.