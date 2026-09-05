<p align="center">
  <img src="https://raw.githubusercontent.com/ManiaxMax/StrataShell-Releases/main/assets/branding/strata-logo.svg" alt="STRATA Shell" width="720" />
</p>

# STRATA Shell

A wallpaper-driven Windows shell with its own desktop, widgets, application bar, workspaces and native settings.

STRATA provides two environments: **Floating** for freely movable, overlapping windows and **Tiled / Center Stage** for one or two arranged applications per workspace. Switch with `Super + Shift + W`. Both share the same applications, wallpaper-derived colors, controls and recovery system.

**Development status:** Phase 5 covers code review, fresh-install defaults and documentation. These pages describe current Preview development; published Stable packages may contain an earlier feature set. See [release and testing status](docs/STATUS.md) and each package's [release notes](https://github.com/ManiaxMax/StrataShell-Releases/releases). Documentation updates do not publish or install a new binary.

## Get STRATA

Download the latest [Stable installer](https://github.com/ManiaxMax/StrataShell-Releases/releases/latest), or select **Preview** in STRATA Settings → Updates for available test releases. Fresh users should start with the complete Stable package; Preview updates are lightweight and reuse a compatible installed desktop runtime.

STRATA is experimental Windows 11 x64 software. Read [installation and recovery](docs/INSTALLATION.md) before making it your sign-in shell. Windows continues to own drivers, secure sign-in, UAC and protected system functions.

## Fresh-install defaults

The next release containing Phase 5 uses **Balanced quality** and **Floating mode** for a brand-new release installation.

| Left column | Right column |
|---|---|
| Clock / Time — locked expanded | YouTube — locked expanded |
| Weather | Audio Spectrum |
| Focus Timer | Performance |
| Notes | AI Command |

Browser and Files are pinned initially. Five workspaces are configured, adjustable from one to ten. The template contains no personal notes, weather location, device identities, account data or selected startup apps.

**These defaults are seeded only when installing without an existing settings file. Updates, repairs and shell startup preserve each user's quality, mode, layout and expansion locks.** An explicit Reset Settings action remains separate. [Default profile and privacy details](docs/SHIPPED_DEFAULTS.md).

## What STRATA includes

| Area | Current Preview implementation |
|---|---|
| Floating desktop | Native title-bar movement, resizing, maximize/minimize, Alt + Tab, grouped dock icons, pins, window previews and normal close requests |
| Center Stage | One full-height or two split applications, per-monitor workspaces, overflow placement, directional movement and window exchange |
| Theme | Wallpaper-derived Light/Dark/Auto palettes; coordinated wallpaper, frost and color transitions; transparency, optional blur/bloom, quality presets and Reduced Motion |
| Widgets | Clock/calendar, Weather, Focus Timer, Notes, Performance, Audio Spectrum, YouTube and AI Command; configurable slots and expansion locks |
| Sound | Output and microphone selection, levels, mute/unmute, application mixer and a speaker-button control popup |
| Network and Bluetooth | Native Wi-Fi discovery/connect/disconnect/forget/radio controls; streaming Bluetooth discovery and supported pairing ceremonies |
| Display and input | Monitor arrangement, resolution, refresh and orientation with timed rollback; mouse/keyboard controls, supported touchpad settings and keybind editing |
| Startup, time and power | Launcher-based startup selections, timezone and clock controls, supported power-plan timeouts, battery information and calendar |
| Applications and tray | App discovery, multi-window STRATA document apps and native publisher tray menus; Open restores existing windows or exposes the publisher's controls |
| Recovery and updates | Immutable releases, validated update archives, Stable/Preview channels, bootstrap watchdog and independent Explorer recovery |

Hardware support depends on Windows, drivers and devices. Per-monitor Windows DPI changes, historical battery usage, immediate Energy Saver switching and Windows Update installation/scheduling are not implemented here. There is no universal tray command that creates every application's missing window. See [features and limitations](docs/FEATURES.md).

## STRATA applications

Browser, Files, Terminal, Text, Snip, Paint, Image Viewer, Media Player and Task Manager share STRATA's visual controls and theme. Browser, Files, Text, Paint, Images, Terminal and Media support independent document windows. Utility surfaces such as Settings remain single-instance tools. [Application guide](docs/FIRST_PARTY_APPS.md).

![STRATA Shell in Tiled dark mode](docs/screenshots/strata-dark.png)

The screenshot illustrates the Tiled composition. Fresh-install Floating defaults and newer controls are described above; it is not a screenshot of the Phase 5 candidate.

## Essential shortcuts

`Super` means the Windows-logo key. The live list respects the active environment and any saved remapping.

| Shortcut | Action |
|---|---|
| `Super + Space` | STRATA Command |
| `Super + Shift + W` | Switch Floating / Tiled |
| `Super + Enter` | Terminal |
| `Super + B` / `Super + F` | Browser / Files |
| `Super + S` | Snip |
| `Super + Ctrl + S` | Settings |
| `Super + K` / `Super + Ctrl + K` | Keybind viewer / editor |
| `Super + Shift + T` | Active-window transparency |
| `Super + 1…0` | Workspace selection |
| `Super + Shift + D` | Floating: show/hide widgets; Tiled: change layout view |
| `Alt + Tab` | Floating app switcher |
| `Ctrl + Alt + Shift + Delete` | Emergency Explorer recovery / exit Preview |

[Complete keybindings](docs/KEYBINDS.md). The STRATA dock icon opens Applications; Super + Escape is retired. Power remains available in the bar.

## Documentation

- [Release and testing status](docs/STATUS.md)
- [Settings](docs/SETTINGS.md) · [Fresh-install defaults](docs/SHIPPED_DEFAULTS.md)
- [Floating mode](docs/FLOATING_MODE.md) · [Features and limitations](docs/FEATURES.md)
- [First-party applications](docs/FIRST_PARTY_APPS.md) · [Screensaver](docs/SCREENSAVER.md)
- [Installation and recovery](docs/INSTALLATION.md) · [Safe shell mode](docs/SAFE_SHELL_MODE.md)
- [Product contract](docs/PRODUCT.md) · [Architecture](docs/ARCHITECTURE.md)
- [Development and verification](docs/DEVELOPMENT.md) · [Contributing](CONTRIBUTING.md)

## Development

Source is maintained privately in `ManiaxMax/StrataShell`. The public `ManiaxMax/StrataShell-Releases` repository contains distribution assets and documentation, not application source. Contributors need access to the private repository.

From an authorized source checkout, install the .NET 9 SDK and build:

```powershell
dotnet restore .\src\StrataShell\StrataShell.csproj
dotnet build .\src\StrataShell\StrataShell.csproj -c Release --no-restore
```

Use `--preview` for coordinated interactive development; do not start a second shell over an active STRATA session. Browser, Terminal and the YouTube widget use the Microsoft Edge WebView2 Runtime. Full packages bundle the .NET desktop runtime; optional AI CLIs and hardware drivers are separate.

Source changes are developed on `preview`; `main` remains the Stable source branch. Documentation can be updated independently and identifies unreleased work explicitly. No open-source license is granted by the public distribution repository; see [contribution and licensing guidance](CONTRIBUTING.md).
