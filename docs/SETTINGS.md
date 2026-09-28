# Settings guide

STRATA Settings saves preferences for the current Windows user, normally under `%LOCALAPPDATA%\StrataShell`. Search at the top of Settings opens the matching page and control. Updates and repairs preserve existing choices; **Reset Settings** is a separate action. See [release status](STATUS.md) for the packages these pages describe.

## Language

Choose one of 25 language/region options or **Follow Windows**. Settings navigation, search and selected shared controls change without restarting STRATA. Translation is partial: many app-specific screens, errors, Setup and Launcher remain in English. Changing STRATA's language does not change Windows or other applications.

## UI & Theme

| Pane | Main controls |
|---|---|
| **Wallpaper** | Choose an image; set Auto, Light or Dark appearance; change fit, scaling, animation, palette and wallpaper grid. |
| **Interface** | Adjust interface typography, glass, blur, bloom, contrast, transparency, motion and performance. |
| **Window Layout** | Select Floating, Tiled or Sphered; configure workspace count, Tiled views, gaps and Floating behavior. |
| **Rail & Dock** | Choose connected rail or separate dock, position, size and visible modules. |
| **Widgets** | Assign widgets to slots and monitors, change titles, visibility and widget-specific options. |
| **Screensaver** | Enable it, change idle delay, respect presentation mode and start a preview. |

New installations start in Floating mode with Balanced quality, five workspaces, Browser and Files pinned, and a five-minute screensaver delay. Existing profiles keep their saved choices. See [desktop modes](FLOATING_MODE.md) and [screensaver](SCREENSAVER.md).

## Windows and device controls

- **Notifications** shows STRATA alerts and recent activity.
- **Display** exposes supported monitor arrangement, resolution, refresh, orientation and brightness controls. Some values are read-only when Windows, hardware or policy does not expose a setter.
- **Sound + Mixer**, **Network** and **Bluetooth** use Windows devices and services. Wi-Fi connection and automatic-connect settings belong to Windows profiles; organization policy or missing permissions can limit changes.
- **Input + Keybinds** shows supported mouse, touchpad and keyboard controls. Open the live shortcut list with **Super + K** and its editor with **Super + Alt + K**. See [keybindings](KEYBINDS.md).
- **Date + Time** reads Windows time and time-zone settings. Manual clock changes require explicit confirmation and Windows elevation; organization-managed time policy remains in force.
- **Startup + App Tray** manages selected startup applications and tray items. It can export a portable startup-app list for another installation.
- **Power + Session** shows supported power and battery controls, lock, sign-out, restart and shutdown actions.

## Updates, recovery and profiles

**Updates** selects Stable or Preview, checks the public release feed without a GitHub account, and verifies a package before installation. Preview is a smaller update and may require a compatible Stable installation or registered .NET 10 Desktop runtime. The running shell is not overwritten; the new release is used at the next sign-in. See [installation and recovery](INSTALLATION.md).

**Recovery + Profiles** offers profile backup/import and recovery of unfinished Notepad, Paint and Snip work where available. **Windows Tweaks** includes the confirmed setting to restore Explorer permanently as the default shell. **Explorer Session** in Power + Session is temporary for the current login. Keep the external Explorer recovery shortcut available.

STRATA controls its own desktop surfaces and delegates protected operations to Windows. A setting marked unavailable or read-only reflects a Windows, device, driver or policy limit; it is not silently applied by STRATA.