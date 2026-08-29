# StrataShell

> A wallpaper-driven Windows shell with a Linux-compositor mindset.

StrataShell replaces the visible Explorer desktop experience with a keyboard-first environment built around a fixed **Center Stage**, persistent side widgets, logical workspaces, wallpaper-derived color, translucent shell surfaces, and recoverable default-shell installation.

Windows remains underneath for application compatibility, drivers, security, and hardware APIs. It is not the visual reference.

![StrataShell in dark mode](docs/screenshots/strata-dark.png)

> [!WARNING]
> StrataShell is experimental alpha software. Preview mode is the recommended development path. Default-shell installation is login-critical: Windows 11 Home/Pro use a reversible current-user shell policy, while supported Enterprise/Education/IoT editions use Windows Shell Launcher. Read [Installation and recovery](docs/INSTALLATION.md) before enabling it.

## What makes Strata different

- **Center Stage instead of a desktop full of windows.** Each workspace contains one full-height centered application or two applications split top/bottom. Side widgets remain anchored across workspace changes.
- **Logical workspaces with high-refresh motion.** Ten workspaces support directional focus, same-layer navigation, animated switching, app movement, and full-workspace slot exchange.
- **Wallpaper as live system state.** Palette extraction drives shell accents, focus borders, graphs, widgets, bloom, console colors, and Windows light/dark preference.
- **A connected shell perimeter.** A slim appbar-reserving top rail and four-sided luminous surround frame the workspace. Quick panels grow from the rail rather than imitating Windows flyouts.
- **Strata-native keyboard grammar.** The Windows-logo key becomes `Super`, with direct launch, focus, movement, workspace, appearance, hardware, and recovery chords.
- **Explorer-independent essentials.** Strata includes application discovery, a launcher, Files, settings, audio, network, Bluetooth, display, session controls, wallpaper management, and direct packaged-app activation.
- **Recovery is part of the architecture.** An installed bootstrap watchdog, an Explorer fallback, an emergency non-`Super` chord, external recovery scripts, and reversible appearance changes are all first-class.

## Screenshots

| Light composition | Top rail |
|---|---|
| ![StrataShell light composition](docs/screenshots/strata-light.png) | ![StrataShell top rail](docs/screenshots/top-rail.png) |

| Live system-audio spectrum |
|---|
| ![Live system audio spectrum](docs/screenshots/audio-spectrum.png) |

The repository includes a curated set of 40 full 4K wallpapers (20 Light, 20 Dark) located under `wallpapers/`, plus the original fallback image under `assets/wallpapers`.

## Current feature set

| Area | Implemented behavior |
|---|---|
| Window management | Per-monitor Center Stage plus wide side-by-side and wide top/bottom views, one/two-app capacity, equal visible width, active theme border, animated focus/swap/view transitions, split glass/true fullscreen, whole-window opacity toggle |
| Workspaces | 1–10 logical workspaces, directional slide transitions, same-layer navigation, move-and-follow, silent move, overflow to the next available workspace |
| Shell surfaces | Wallpaper desktop, top rail, edge surround, launcher, quick panels, app-tray overflow, OSD, keybind viewer, icon-guided Settings, Strata Files, Strata Terminal, Strata Text, Strata Snip |
| Theme | Automatic/dark/light modes, wallpaper palette extraction, default-on Windows lock/sign-in wallpaper sync, Windows theme sync, CMD/PowerShell palette sync, vibrancy, glass opacity, blur, CRT bloom, high contrast, reduced motion |
| Widgets | Weather, clock/calendar, focus timer, performance/process monitor, WASAPI system-audio spectrum, multi-provider AI CLI, embedded YouTube search/player |
| Hardware/session | Master volume, mute, in-shell output selection, microphone mute, media controls, brightness where exposed, live Wi-Fi discovery/connect/disconnect, Bluetooth discovery/pair/connect/disconnect/unpair, lock/sleep/hibernate/sign out/restart/shutdown |
| Applications | Win32 shortcuts and registered executables, packaged apps, optional native/default icons, Strata Terminal, Strata Text, Strata Snip, external PowerShell, browser forced to a separate window, direct ChatGPT activation |
| Background apps | One first-position down-chevron opens deduplicated app-published tray icons with each app's native Open/Menu actions; tray-minimized windows leave the tiler, while non-tray background apps expose confirmed Show/Exit actions |
| Files | Editable path, debounced search, Explorer-style type-to-select, virtualized Details/Icon views, visible-first cached media thumbnails, themed Strata file picking, Known Folder and redirected/network locations, navigation history, hidden-file toggle, properties, clipboard operations, drag/drop, rename/new folder, Recycle Bin |
| Screensaver | Full-screen per-monitor Strata signal renderer after five idle minutes, fifteen fast theme-colored modes, presentation deferral, reduced-motion/high-contrast paths, any-input dismissal, manual preview |
| Updates | Stable and Preview channels, authenticated private GitHub release discovery, Windows Credential Manager storage, verified bounded downloads, and immutable self-tested installation |
| Safety | Comprehensive self-test suite, versioned local installs, 15-minute Windows idle floor, edition-aware Home/Pro or Shell Launcher activation, external Explorer recovery tools, three-failure watchdog |

See [Implemented features and limitations](docs/FEATURES.md) for the precise current boundary.

## Center Stage model

Strata deliberately does not reproduce a conventional free-floating Windows desktop.

| Workspace contents | Layout |
|---|---|
| 1 app | The app fills the complete centered lane, aligned to the visible top and bottom widget-card edges. |
| 2 apps | The first app occupies the top slot and the second occupies the bottom slot at equal width. |
| Open a third app | The new app is assigned to the next workspace with capacity, and Strata follows it. |
| Move into a full workspace | The incoming app takes the matching slot; the displaced app returns to the source workspace. |
| Floating | `Super + W` detaches every app on the active workspace from its tile, restores each app's last floating geometry, and gives the active float visual precedence: app windows and widget cards disappear only where it overlaps them. `Ctrl` + left-drag anywhere inside a floating window moves it. Workspace switching preserves that geometry; opening or moving another app into the workspace returns it to tiled mode. |
| Glass-expanded | The active app retains transparency/effects, fills the work area with a reveal gap, and temporarily fades widgets. |
| True fullscreen | The active app becomes opaque, takes the monitor, and hides the rail until the pointer touches the top edge. |

The rendered widget envelope is measured at runtime. Changing **Settings → UI & Theme → UI Scale** keeps both one-app and two-app layouts aligned with the visible widget edges. The top rail also ducks whenever a managed window crosses its screen area and returns when the overlap clears.

## Requirements

### Preview and development

- Windows 11 x64
- [.NET 9 SDK](https://dotnet.microsoft.com/download/dotnet/9.0)
- Microsoft Edge WebView2 Runtime for the embedded YouTube widget

### Optional integrations

- ChatGPT desktop app for direct `Super + C` activation and the corresponding installed self-test
- `agy`, `codex`, and/or `claude` executables for the AI CLI widget
- A brightness interface exposed through WMI for software brightness control

### Default-shell mode

- Windows 11 Home, Pro, Enterprise, Education, or IoT Enterprise
- Administrator approval only when the selected edition uses Windows Shell Launcher
- A local self-contained Strata release; UNC/network executables are rejected

## Run a safe preview

```powershell
git clone https://github.com/ManiaxMax/StrataShell.git
cd StrataShell
dotnet restore .\src\StrataShell\StrataShell.csproj
dotnet run --project .\src\StrataShell\StrataShell.csproj -- --preview
```

Preview mode shows the shell without changing the configured Windows shell. Use `Ctrl + Alt + Shift + Delete` to exit preview if needed.

Additional startup modes and acceptance commands are documented in [Development and testing](docs/DEVELOPMENT.md).

## Build

```powershell
dotnet build .\src\StrataShell\StrataShell.csproj -c Release
```

Run the noninteractive installed-environment checks:

```powershell
.\src\StrataShell\bin\Release\net9.0-windows10.0.26100.0\win-x64\StrataShell.exe --self-test --quiet
```

The self-test writes its JSON report to `%LOCALAPPDATA%\StrataShell\Recovery\self-test.json`. Some checks are intentionally machine-dependent, including wallpaper libraries and the ChatGPT packaged-app identity.

## Wallpaper libraries
 
Strata ships with a curated collection of 40 full 4K wallpapers organized into paired Light and Dark libraries:
 
```text
wallpapers/
├── WallpapersDark/   (20 4K abstract/geometry dark wallpapers)
└── WallpapersLight/  (20 4K abstract/geometry light wallpapers)
```
 
- `Ctrl + Super + Up` selects the Light collection and light application theme.
- `Ctrl + Super + Down` selects the Dark collection and dark application theme.
- `Ctrl + Super + Left/Right` moves backward/forward in the active collection.
 
The picker provides search, pagination, thumbnails, and a full preview. Its **Browse file** action opens Strata Files in selection mode. The library is watched recursively: adding, deleting, or replacing a supported image refreshes both the chooser and cycling shortcuts without restarting Strata.

## Essential shortcuts

| Shortcut | Action |
|---|---|
| `Super + Space` | Command launcher |
| `Super + Enter` | Strata Terminal |
| `Super + Ctrl + Enter` | PowerShell |
| `Super + Ctrl + S` | Strata Snip |
| `Super + B` | New browser window |
| `Super + F` | Strata Files |
| `Super + S` | Settings |
| `Super + Shift + S` | Start the Strata screensaver immediately |
| `Super + -` | Remove the highest workspace when it is empty (minimum 1) |
| `Super + +` | Add a workspace (maximum 10) |
| `Super + A` | Antigravity |
| `Super + C` | ChatGPT / Codex |
| `Super + K` | Searchable keybind list |
| `Super + T` | Toggle transparency for the active third-party or Strata window |
| `Super + W` | Toggle every app on the active workspace between tiled and floating |
| `Super + D` | Cycle the active monitor through Center Stage, wide side-by-side, and wide top/bottom |
| `Super + \` | Glass-expanded / return to Center Stage |
| `Super + Shift + \` | True fullscreen / return to Center Stage |
| `Super + Arrow` | Focus the next existing window in the bounded spatial path |
| `Super + Shift + Arrow` | Move/fill/swap the active app in that direction |
| `Super + 1…0` | Switch workspace 1…10 |
| `Ctrl + Alt + Shift + Delete` | Exit preview or restore Explorer from shell mode |

The exact 94-binding policy is documented in [Keybindings](docs/KEYBINDS.md).

## Default-shell installation

Do not enable shell mode before testing preview, ChatGPT/terminal launch, and Explorer recovery on the target machine.

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\scripts\Install-StrataShell.ps1 -Activate -ActivationMode Auto
```

The installer publishes a self-contained, versioned release beneath `%LOCALAPPDATA%\Programs\StrataShell`, scans and initializes portable state, runs its self-test, writes recovery tools beneath `%USERPROFILE%\Strata Recovery`, and selects the correct Home/Pro policy or Shell Launcher route automatically. `scripts\Build-InstallerBundle.ps1` creates a self-contained ZIP whose target PC does not need the .NET SDK.

Read the full [installation, update, and recovery guide](docs/INSTALLATION.md) first.

## Packaged release installation
 
Strata provides self-contained single-file installers (`StrataShell-Setup-<version>-win-x64.exe`) and portable ZIP release bundles (`StrataShell-<version>.zip`) published on [GitHub Releases](https://github.com/ManiaxMax/StrataShell-Releases).
 
- Includes the self-contained .NET 9 desktop runtime.
- Bundles all 40 full 4K curated dynamic Light/Dark wallpapers.
- Configures Strata Launcher, Start Menu shortcuts, uninstaller, and external Explorer recovery tools.
- Supports edition-aware activation for Windows 11 Home, Pro, Enterprise, Education, and IoT Enterprise.
- Provides seamless, silent background updates via **Settings → Updates** with automatic bootstrap version delegation upon sign-in.
- Shipped defaults use the privacy-safe schema-23 allowlist documented in [SHIPPED_DEFAULTS.md](docs/SHIPPED_DEFAULTS.md).
 
```powershell
.\scripts\Build-StrataInstaller.ps1 -Version '<version>'
.\scripts\Test-StrataInstaller.ps1
```

## Repository map

```text
assets/                    Bundled fallback wallpaper, branding, and local YouTube host
docs/                      Product, architecture, settings, operation, and screenshots
installer/                 Privacy-scrubbed defaults for packaged installations
scripts/                   Publish, installer, validation, edition-aware shell, and recovery tools
src/StrataLauncher/        Explorer-mode maintenance and shell launcher utility
src/StrataMaintenance/     Shared install, update, repair, reset, and restore logic
src/StrataSetup/           Self-contained graphical installer and uninstaller
src/StrataShell/           WPF application and Win32/DWM integration
wallpapers/                Ignored personal Light/Dark libraries and setup notes
```

Key documents:

- [Implemented features and limitations](docs/FEATURES.md)
- [First-party applications](docs/FIRST_PARTY_APPS.md)
- [Settings reference](docs/SETTINGS.md)
- [Keybindings](docs/KEYBINDS.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Screensaver](docs/SCREENSAVER.md)
- [Installation and recovery](docs/INSTALLATION.md)
- [Development and testing](docs/DEVELOPMENT.md)
- [Product contract](docs/PRODUCT.md)
- [Reference research](docs/REFERENCE_RESEARCH.md)
- [Contributing](CONTRIBUTING.md)
- [Security policy](SECURITY.md)

## Important boundaries

- Strata does not intercept `Ctrl + Alt + Delete`, UAC secure desktop, sign-in, Windows Recovery, or other Windows-owned security surfaces.
- Application transparency is whole-window alpha. Strata cannot force arbitrary third-party toolkits to render their internal materials as acrylic.
- Blur, CRT bloom, and spring/wobble effects are guaranteed for first-party shell surfaces. True mesh deformation of unrelated application pixels is not implemented.
- Microsoft Media Player uses a single-stage opaque compatibility path so auxiliary playback/control surfaces do not consume workspaces or lose hardware video composition. Other protected/video windows may still reject DWM style, border, chrome, or opacity changes.
- Ordinary Wi-Fi, Bluetooth, and audio workflows stay inside Strata. Hardware-required pairing consent/PIN, drivers, display topology, UAC, and notification permissions remain protected Windows surfaces.
- The YouTube widget opens account sign-in and management in the default browser; credentials are not entered into or retained by the embedded player.

## License

No open-source license has been selected yet. Public source visibility alone does not grant reuse, redistribution, or derivative-work rights beyond GitHub's Terms of Service. Add an explicit `LICENSE` before accepting outside redistribution.
