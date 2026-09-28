# Features and limits

This guide describes the current published STRATA Shell packages. See [release status](STATUS.md) for the exact Stable and Preview versions and what has been verified. STRATA is an experimental Windows 11 desktop shell; an automated package check is not the same as acceptance on an installed desktop.

## Desktop modes

| Mode | What it does | Main limit |
|---|---|---|
| **Floating** | Move, resize, overlap and minimize ordinary windows; switch them with the dock or Alt + Tab. It is the fresh-install default. | Some applications control their own window chrome or must remain opaque. |
| **Tiled** | Center Stage arranges one or two apps. Wide and Dynamic views provide other layouts, including additional tiled apps. | Windows with hard minimum sizes may not fit every pane. |
| **Sphered** | Collect apps, web pages and settings in one Sphere per monitor, with tabs and optional two-pane groups. | Native apps, games and protected windows have [compatibility limits](SPHERED.md). |

Each monitor has its own active workspace and layout state. A fresh installation has five workspaces; the setting supports up to ten. Use **Super + Ctrl + D** for Floating/Tiled, **Super + Ctrl + S** for Sphered, and **Super + Ctrl + W** for the active mode's views or widgets. See [Floating and Tiled](FLOATING_MODE.md) and [keybindings](KEYBINDS.md).

## Wallpaper and appearance

The active wallpaper supplies STRATA's color palette. Choose Auto, Light or Dark appearance and adjust glass, blur, bloom, transparency, motion and rendering quality in Settings. Stable 1.0.23 includes 24 approved 4K wallpapers: 12 colors in Light and Dark. Preview updates do not include the wallpaper library. The screensaver uses the same theme palette; see [screensaver behavior](SCREENSAVER.md).

Eligible third-party applications receive whole-window opacity and compatible chrome changes. STRATA cannot turn their internal controls into glass. Reduced Motion and High Contrast change visual effects. Some fullscreen games suppress shell overlays and keep their own rendering.

## Widgets and applications

The desktop offers Clock/Calendar, Weather, Focus Timer, Notes, Performance, Audio Spectrum, YouTube and AI Command widgets. Widget availability can depend on network access, media playback, hardware counters or an installed AI command-line provider.

First-party apps include Browser, Files, Terminal, Notepad, Snip, Paint, Image Viewer, Media Player, Calendar and Task Manager. See the [app guide](FIRST_PARTY_APPS.md) for capabilities and limits. STRATA Files can open Windows' native Security permissions page for physical files and folders; Windows performs permission editing and elevation.

## Windows boundaries and recovery

Windows still owns sign-in, UAC, the secure desktop, drivers, protected system surfaces and application frameworks. STRATA does not replace every Explorer namespace, shell extension or Windows Settings control. Hardware and third-party behavior can vary by device and application.

The installer uses a self-test, an external Explorer recovery shortcut and a startup watchdog. Review [installation and recovery](INSTALLATION.md) before making STRATA the sign-in shell. Downloaded executables do not yet have Windows Authenticode publisher signatures; update archives use separate STRATA verification.

Language support covers 25 language/region choices plus Follow Windows in Settings and selected shared controls. App-specific screens, errors, Setup and Launcher may remain in English. See [Settings](SETTINGS.md) for scope.