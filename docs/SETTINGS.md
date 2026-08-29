# Settings reference

Strata Settings is a first-party themed surface. Changes are saved per user and most visual controls apply live. Theme-colored Fluent icons identify navigation categories, destinations, and direct actions; toggles and sliders remain visually quiet so the control state stays primary. Settings are stored beneath `%LOCALAPPDATA%\StrataShell`; generated state is not committed to Git.

## UI & Theme

The visual controls are consolidated into one section with six focused panes. Sliders stay together for continuous values, while binary choices use theme-aware OFF/ON toggles.

### Wallpaper

- Select Dark, Light, or Auto mode.
- Browse the active wallpaper library or open the full wallpaper chooser.
- Adjust vibrancy, glass opacity, blur, CRT bloom, and animation speed.
- Auto mode evaluates wallpaper luminance and selects the matching Windows and Strata application mode.
- Light and Dark collections preserve their most recently selected wallpaper independently.

### Interface

- Scale the complete desktop UI from its native 1.00x size up to 1.35x; underscaling is intentionally disabled.
- Toggle native window and menu shadows independently from glass transparency.
- Enable high contrast or reduced motion.

UI Scale changes both widget columns and the Center Stage envelope. One-app and two-app layouts remain aligned to the rendered top and bottom widget-card edges rather than a fixed screen percentage.

### Top Bar + Surround

- Adjust rail height, top inset, and side inset.
- Enable or disable workspaces, context, CPU, active-app information, network, Bluetooth, audio, battery, application tray, power, launcher, and Settings modules independently.
- Enable hover panels and tune their activation delay.
- The rail reflows automatically as modules are changed.
- The application-tray control is a down-chevron at the left edge of the right-side status group, immediately before the ordinary connectivity/status controls.
- Configure the themed surround thickness and intensity. A thickness of `0 px` turns the four-edge surround off completely.

### Widgets

- Globally show or hide desktop widgets and system telemetry.
- Enable weather, clock, focus timer, performance, audio spectrum, AI CLI, and YouTube widgets independently.
- Use Additional Widget Transparency from 0-100% and adjust hover-expansion delay. Zero preserves the normal fully opaque widget appearance.
- Open the shared Fluent gear in the top-right of every widget for settings local to that surface.
- Weather controls location, Fahrenheit/Celsius, manual refresh, and 5/12/30/60-minute refresh cadence.
- Time + Calendar controls 12/24-hour time, seconds, and Sunday/Monday week start.
- Focus Timer controls default/custom duration, notifications, completion sound, and reminder text while keeping quick presets in the expanded card.
- AI Command controls provider, working directory, conversation continuation, tool-activity visibility, and session reset.
- Performance controls visible CPU/memory/network/process modules and 1/2/5/10-second telemetry cadence.
- Audio Spectrum controls system-mix or microphone capture, band/mirror/level rendering, and sensitivity. Samples remain in memory and are never saved.
- YouTube controls account access through the default browser, result thumbnails, the home button, autoplay, and preferred playback speed. Stream quality stays automatic because YouTube no longer exposes a working quality override.

### Window Layout

- Adjust the Center Stage width ratio and maximum width.
- Adjust inner and outer gaps.
- Adjust active-border thickness.
- Set application transparency policy and opacity.
- Set inactive-window opacity.
- Enable the themed active-window border.
- Enable shell-surface spring/wobble motion.
- Native windows that meaningfully reject repeated tile placement are promoted to floating instead of being trapped in a resize loop. A floating window takes visual precedence: only the tiled windows and widgets it overlaps are hidden, then restored when the overlap ends or the float closes.

`Super + D` cycles the active monitor independently through Center Stage, wide side-by-side, and wide top/bottom. Secondary displays initially use wide side-by-side without widgets. Old freeform and recursive tiling modes are not retained.

### Files + Launcher

- Use the launcher's top-right gear to choose between uniform Strata glyphs and original application icons.
- Use the Strata Files top-right gear to enable or disable lazy image, video, and PDF thumbnails in icon view.

## Notifications

- Enable or disable normal Strata notifications.
- Enable Do Not Disturb for noncritical banners.
- Critical hardware and shell-recovery signals remain separate from optional banners.
- Windows per-application notification permissions remain app-owned. Strata deliberately does not launch that protected `ms-settings` page because Windows can reject or stall it while Explorer is not the active shell.

## Network

- See whether the active route is Wi-Fi, wired LAN, another adapter, or offline.
- Scan available networks with live signal, security, saved-profile, and connected state.
- Connect saved networks with one click, enter a password for a new secured network, connect open networks, or disconnect—all inside Strata.

Strata stores new credentials through the current user's Windows WLAN profile store; the temporary enrollment document is deleted immediately after the profile is added.

## Bluetooth

- Discover paired and currently discoverable Bluetooth Classic/LE devices.
- Pair or request a connection directly from Settings or the top-bar panel.
- Disconnect a connected device, reconnect a paired device, or unpair a saved device from either surface.

Windows can still show a protected PIN or consent handshake when the device's pairing ceremony requires one.

## Volume + Audio

- Adjust master output volume and mute state.
- Select the default playback device for media, applications, and communications.
- Use the same output picker from the top-bar audio panel or expanded spectrum widget.

## Display
 
- View primary workspace resolution, scale/DPI metrics, and virtual desktop dimensions.
- Adjust display luminance slider when the display hardware exposes a supported software brightness channel.
 
## Input + Keybinds

- Review the core shortcut grammar.
- Open the complete searchable keybinding viewer.
- Confirm that `Super` means the Windows-logo key.
- Review the non-`Super` emergency recovery chords.

The current build uses a fixed Strata-native binding policy. User remapping is a future capability, not a current setting.

Workspace Count accepts 1 through 10. `Super + -` decreases the same saved value only when the highest workspace is empty; `Super + +` increases it. The rail and an open Window Layout pane refresh from that setting.

## Windows Tweaks

- Hide native caption controls where the target window supports it.
- Apply the themed active-window border.
- Apply wallpaper-derived caption and title-text colors to compatible native DWM title bars; this reversible tweak is enabled by default.
- Synchronize Windows light/dark application preference.
- Synchronize each Strata wallpaper change to the current user's Windows lock screen by default.
- Enable or disable future lock/sign-in wallpaper synchronization independently; disabling preserves the current lock-screen image.
- Open the Windows sign-in-background option that determines whether the lock-screen picture is reused behind sign-in.
- Theme Command Prompt and PowerShell palettes.
- Enable external theme adapters.
- Toggle application-window transparency support.
- Make Strata the default shell or restore Explorer through the edition-aware route: current-user policy on Windows 11 Home/Pro, Shell Launcher on supported Enterprise/Education/IoT editions.

Each compatibility tweak is saved independently and is designed to be reversible. Applications may ignore unsupported DWM or chrome operations.

## Startup + Background Apps

- Review startup registrations discovered from the supported per-user and machine startup locations.
- Use the top-rail tray dropdown for app-published controls such as Tailscale's own menu instead of adding one icon per application to the rail.
- Published tray entries retain their real application icon and expose separate Open and Menu actions.
- Registrations are grouped into one base row per product/company; distinct publisher controls remain available from that row's submenu, and shell hardware pseudo-icons are excluded.
- Export or import a portable startup manifest. First run reconciles a supplied manifest once, preserves existing choices, rebases registered executable paths where possible, and skips missing/unsafe entries.
- An application that closes its visible window into the tray is removed from Center Stage without terminating its background process.
- Background applications without a published tray entry expose Show and a separately confirmed Exit action when Strata can identify their process.

## Power + Session

- Use the aligned two-column icon grid for clear session and power actions.
- Lock immediately.
- Sleep or hibernate.
- Sign out.
- Restart or shut down.
- Restart Strata or open a temporary Explorer Session when running as the shell. Explorer Session closes Strata and starts the normal Windows desktop for the current login without changing the configured default shell; Strata returns after sign-out or restart.
- Permanently restore Explorer only from **Windows Tweaks → Default Shell**. The setting explains that it changes future sign-ins, requests confirmation, and uses the edition-appropriate elevated Shell Launcher or per-user-policy recovery route.

Actions that end or replace the current session—including Explorer Session—require confirmation; lock does not. The emergency `Ctrl + Alt + Shift + Delete` recovery chord remains a permanent Explorer restore rather than a temporary session switch.

## Screensaver

- Enable or disable automatic idle activation.
- Choose an idle delay from 1 to 60 minutes; the shipped default is 5 minutes.
- Defer automatic activation only during Windows presentation mode or full-screen Direct3D activity. The generic Windows "busy" notification state does not block an otherwise idle five-minute launch.
- Start the real full-screen surface immediately with the preview route or `Super + Shift + S`.
- Inspect the live Strata state and the Windows 15-minute idle-floor verification status.

The screensaver uses the active wallpaper theme—including distinct light and dark fields—and the global High Contrast and Reduced Motion choices. Any mouse movement, click, wheel action, touch/stylus input, or keyboard key dismisses it without leaving the global hotkey router's modifier state stuck.

## Updates

- Choose Stable for finished releases or Preview for the newest available release, including prereleases.
- Connect each Windows account to the private GitHub repository once. The token is stored in Windows Credential Manager, never in `settings.json`, logs, diagnostics, or process arguments.
- Repository owners can use a read-only fine-grained token. Accepted collaborators currently use their own classic token with repository access because GitHub does not support fine-grained personal tokens for repository-collaborator access.
- Check for updates without downloading anything, then use Update Now to download, validate, self-test, and install the selected immutable release.
- The running shell is not replaced in place. An activated installation begins using the new release at the next sign-in, preserving the existing Explorer recovery path.

Downloaded archives must use the `StrataShell-*.zip` release naming policy. Strata enforces compressed and expanded size limits, rejects traversal paths, verifies GitHub's SHA-256 digest when supplied, requires the expected payload/installer structure, and invokes the normal edition-aware installer.

## About

- View a theme-reactive Strata logo and fetch-style system summary.
- Inspect host, Windows build, the current timestamped shell release, engine version, theme, uptime, CPU, GPU, memory, displays, architecture, and runtime at a glance.
- See whether the Explorer failsafe is installed and open project or recovery locations where available.

## Settings schema

`ShellSettings` has a versioned schema (`SchemaVersion 23`) and defaults. Unknown or missing values fall back through the settings service rather than preventing shell startup. Local values include paths and machine preferences, so `settings.json` is intentionally ignored by Git.
