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

## Restore and build

```powershell
dotnet restore .\src\StrataShell\StrataShell.csproj
dotnet build .\src\StrataShell\StrataShell.csproj -c Release
```

The project treats compiler warnings as errors. Generated `bin` and `obj` trees are ignored by Git.

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

The current suite covers 52 policies/integrations, including:

- required install and recovery paths;
- settings parsing and persistence;
- wallpaper libraries and fallback assets;
- packaged ChatGPT discovery;
- keybinding uniqueness and registration policy;
- one/two-app Center Stage geometry;
- widget-column and vertical-envelope calculations;
- frame compensation and chrome policy;
- workspace overflow and new-window placement;
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
8. Fullscreen becomes opaque and rail edge reveal works.
9. Launcher/Settings/Files receive keyboard focus immediately.
10. Light and dark wallpapers update shell and Windows app-theme preference.

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

## Publishing locally

Create a self-contained release without changing shell configuration:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\scripts\Install-StrataShell.ps1
```

See [Installation and recovery](INSTALLATION.md) for activation. Never point Shell Launcher at a mutable development build.
