# Settings reference

STRATA Settings is a first-party themed surface. Changes are saved per user and most visual controls apply live. Theme-colored Fluent icons identify navigation categories, destinations, and direct actions; toggles and sliders remain visually quiet so the control state stays primary. Settings are stored beneath `%LOCALAPPDATA%\StrataShell`; generated state is not committed to Git.

Schema **36** adds `WindowManagementMode` (0 = Tiled, 1 = Floating) and `DockPins`. Existing profiles migrate to Tiled with Browser and Files pinned; an intentionally empty pin list stays empty. The STRATA launcher is permanent and is not a removable pin. Mode can be changed in **UI & Theme → Interface → Window Management Mode** or with `Super + Shift + W`. Tiled layout settings are hidden while Floating is active, and the title-bar-control preference is retained but overridden so Floating apps expose their controls.

Floating customization in that section includes background transparency (`FloatingBackgroundOpacity`, default 0.20, range 0.02–1), desktop-click selection clearing (`FloatingDesktopClickMinimizes`, retained for backward-compatible settings storage and defaulting to true), and dock height (`FloatingDockHeight`, default 48 DIPs, range 44–72). Background dimming applies only to inactive apps overlapped by the active app. It multiplies their configured opacity and restores the baseline on activation or when the active app moves away. Covered widgets become completely transparent. These preferences persist independently of tiled settings. Bar customization also adapts its height and edge-inset labels and hides tiled context controls in Floating mode.

Schema 36 changes the background-transparency default from 90% to 80%. Existing schema-35 profiles using the previous 0.10 opacity default migrate to 0.20; other saved customization is retained. Reduced Motion continues to disable workspace animation; with it off, both Tiled and Floating workspaces swipe horizontally.

Schema 37 changes the Floating dock default from 50 to 48 DIPs. Existing profiles still using the former 50-DIP default migrate to 48; customized dock heights are preserved.


Schema 38 adds `DisplayLayouts`, a bounded dictionary of SHA-256 display-identity keys holding the selected/previous workspace, view mode and widget visibility. It contains no window handles, app sessions or window contents. Existing appearance and app settings survive migration; fresh packages ship an empty dictionary. The legacy `DisableUnusedWindowsProcesses` flag is ignored and normalized to false. `ScreensaverWindowsIdleFloorSeconds` is compatibility data only and no longer changes Windows. Settings uses compact navigation below 920 DIPs and dialogs fit their current display work area.

## UI & Theme

The visual controls are consolidated into one section with five focused panes. Sliders stay together for continuous values, while binary choices use theme-aware OFF/ON toggles. Window Layout is part of Interface so global scale, material, surround, and geometry can be tuned in one place.

### Wallpaper

- Select Dark, Light, or Auto mode.
- Browse the active wallpaper library or open the full wallpaper chooser.
- Set **Wallpaper Scaling** to `High Quality` (Bicubic) or `Fast / Linear`.
- Set **Wallpaper Fit** to `Fill (Crop)`, `Fit (Entire)`, or `Stretch (Full)`.
- Toggle the optional wallpaper grid. Wallpaper-derived color tuning lives with the rest of the shared interface material controls.
- Auto mode evaluates wallpaper luminance and selects the matching Windows and STRATA application mode.
- Light and Dark collections preserve their most recently selected wallpaper independently.

### Interface

The Interface pane is organized into focused thematic subjects:

#### Graphics & Performance
- Select **Performance Mode**: `High`, `Balanced`, `Low`, or `Custom`. `High` preserves full visual fidelity; `Balanced` keeps full-motion, monitor-aware animation and high-quality wallpaper scaling while disabling bloom, frost backdrops and shadows; `Low` also reduces motion, uses linear wallpaper scaling and caps frame rate at 60.
- Set **Max Animation Frame Rate**: `Native / Uncapped`, `60 FPS Cap`, or `30 FPS Cap`.
- Toggle **Auto Low Effects on Battery** to automatically reduce GPU fill-rate and effects workload when operating on battery.
- Percentage controls place `50%` at their physical midpoint, and neutral scale/speed controls place `1.00×` at their midpoint even when the safety range is asymmetric.

#### Display & Scale
- STRATA Shell detects the active workspace tier—720p, 1080p, 1440p, 3K, or 4K+—and highlights its recommended composition baseline.
- Adjust display scale from 0.85× to 1.35× around that recommendation, or choose **Use Recommended** to return the adjustment to 1.00×. The displayed effective scale combines the recommendation and adjustment.
- Display Scale changes both widget columns and the Center Stage envelope. One-app and two-app layouts remain aligned to the rendered top and bottom widget-card edges rather than a fixed screen percentage.

#### Color & Lighting
- Adjust wallpaper-derived **Vibrancy** (saturation range for interface elements).
- Adjust **Bloom** (phosphor light spill around accent labels, active controls, and surround). Bloom is palette-wide rather than a single accent shadow: primary/secondary signals, warning/success/danger states, colored text and icons, borders, status dots, graphs, and other colored control graphics receive color-matched phosphor halos. Neutral body text and neutral surfaces remain crisp.
- Configure themed workspace-surround **Intensity** and **Thickness**. A thickness of `0 px` turns the four-edge surround off completely.
- Toggle **High Contrast** to increase visual separation while retaining the wallpaper palette identity.

#### Glass, Blur & Transparency
- Adjust **Glass Transparency** from `0%` (solid) through the `65%` readability limit for first-party surfaces. Existing saved opacity values are displayed through the inverse control without changing their visual result.
- Adjust **Additional Widget Transparency** directly below the shared glass control.
- Adjust **Frosted Glass Blur** continuously from clear through strong frost: STRATA builds one cached, wallpaper-aligned Gaussian frost beneath the existing tint layer while DWM remains clear.
- Toggle **STRATA Surface Frosted Glass** and **Widget Frosted Glass** independently to choose which targets consume the global blur strength. Wallpaper, palette and monitor-sized frost prepare before publication; wallpaper and frost start together with the same crossfade duration/easing. Strong frost filters at half resolution before restoring the original texture dimensions; wallpaper resolution is unchanged.

#### Depth & Shadows
- Toggle native **Window Shadows** and **Widget Shadows** independently from glass transparency.

#### Motion & Transitions
- Adjust **Motion Speed** multiplier for transitions and workspace slides.
- Toggle **Reduced Motion** to replace spatial transitions with immediate state changes.

### Top/Bottom Bar

- Adjust the active bar height, edge inset, and side inset. Reset Bar Size & Spacing resets only those dimensions.
- Enable or disable workspaces, context, active-app information, network, audio, battery, application tray, Snip, and Settings modules. Tiled mode also exposes the Command launcher module.
- Power is always visible. Bluetooth has its own working status/tray button beside Network. The CPU module and the old, unused Bluetooth visibility option remain removed.
- In Floating mode, the STRATA dock icon opens Applications. Dock height is configured here.
- The rail reflows automatically as modules are changed.
- Each connected monitor receives its own top bar, independent local workspace indicator, active-app context, and reserved work area. The rail currently owning workspace hotkeys shows **Active Monitor** after its workspace numbers.
- The application-tray control is a down-chevron at the left edge of the right-side status group, immediately before the ordinary connectivity/status controls.

### Widgets

- The master Widget Layer, Expand on Hover, title visibility, and expansion-delay controls are grouped above the individual modules.
- Enable weather, clock, focus timer, performance, audio spectrum, notes, AI CLI, and YouTube widgets independently.
- Hold Ctrl and drag to move or swap any of the 16 slots. Crowded columns scroll; holding a dragged card at a column edge scrolls to additional slots. Expanded controls retain their usable height. The lock button cycles expanded, collapsed, and unlocked.
- Open the shared Fluent gear in the top-right of every widget for settings local to that surface.
- Weather controls location, Fahrenheit/Celsius, normal/themed icons, manual refresh, and 5/12/30/60-minute refresh cadence. Recognizable normal weather icons are the default; the STRATA glyph treatment remains optional.
- Time + Calendar controls 12/24-hour time, seconds, and Sunday/Monday week start; the compact date uses the full weekday/month form without slash separators.
- Focus Timer controls default/custom duration, notifications, completion sound, and reminder text while the card exposes progress, remaining percentage, pause/reset, and a quick five-minute extension.
- AI Command controls provider, working directory, conversation continuation, tool-activity visibility, and session reset.
- Performance controls visible CPU/memory/network/process modules and 1/2/5/10-second telemetry cadence. CPU and RAM values include color keys matching their graph lines.
- Audio Spectrum controls system-mix or microphone capture, band/mirror/level rendering, and sensitivity. Samples remain in memory and are never saved.
- YouTube controls account access through the default browser, result thumbnails, the home button, autoplay, and preferred playback speed. Its compact copy avoids repeating YouTube/Search/Ready labels, while stream quality stays automatic because YouTube no longer exposes a working quality override.

### Window Layout within Interface

- Adjust the Center Stage width ratio and maximum width.
- Adjust inner and outer gaps.
- Adjust active-border thickness.
- Enable the themed active-window border.
- Native windows that meaningfully reject repeated tile placement are promoted to floating instead of being trapped in a resize loop. A floating window takes visual precedence: only the tiled windows and widgets it overlaps are hidden, then restored when the overlap ends or the float closes.

`Super + Shift + D` cycles the active monitor independently through Center Stage, wide side-by-side, and wide top/bottom. Mixed native/STRATA transitions are driven from WPF's presented composition frames as one geometry batch; first-party frost and bloom work pauses during motion and resumes after the exact final tile, preventing STRATA apps from lagging behind native apps. `Super + Alt + 1…4` selects a monitor; `Super + Alt + Shift + 1…4` moves the active app into that monitor's currently visible workspace. Secondary displays initially use wide side-by-side without widgets, but retain their own top bar and dedicated workspace state. Old freeform and recursive tiling modes are not retained.

### Screensaver

- Open the full Screensaver controls from the UI & Theme route. The launcher and STRATA Files retain their own top-right gear controls instead of occupying a separate Settings pane.

## Notifications

- Enable or disable timer completion alerts and their configured sound. The timer widget also has its own Notify on Complete setting.
- Critical hardware and shell-recovery signals remain separate from optional banners.
- Windows per-application notification permissions remain app-owned. Use the Explorer companion route to open native notification settings.

## Network

- See whether the active route is Wi-Fi, wired LAN, another adapter, or offline.
- Scan available networks with live signal, security, saved-profile, and connected state.
- Connect saved networks with one click, enter a password for a new secured network, connect open networks, or disconnect—all inside STRATA Shell.
- Use the radio toggle to enable/disable Wi-Fi, or expand Saved Connections to forget a profile after confirmation, including networks that are out of range. Organization-managed profiles cannot be removed here.
- Click the network status icon to open the same native controls used by Settings. Saved/unsaved observations of one network are consolidated without mixing adapters or security types.

STRATA Shell enrolls credentials directly in memory into the current user's Windows WLAN profile store. No temporary password file is created. Hidden/enterprise and unsupported enrollment types need an existing Windows profile; unknown authentication never becomes an open connection. Cancel stops the current attempt. Windows Location permissions or organization policy can deny network access; STRATA reports the reason and does not alter those permissions.

## Bluetooth

- Discover paired and nearby Classic/LE devices as live cards. Devices appear during scanning; names, pairing and connection state update in place.
- Use Find Devices with a mouse, keyboard, headset, speaker or other peripheral in pairing mode. The list is shared by Settings and the Bluetooth tray button.
- Toggle the Bluetooth radio, pair a device, request a connection, disconnect, or remove a pairing. Identical friendly names do not merge separate device identities.
- STRATA handles confirmation-only, keyboard PIN display, PIN entry, and matching-code confirmation through Windows' supported custom-pairing API. Cancelling does not accept the ceremony.

Normal Wi-Fi and Bluetooth workflows do not require starting Explorer. Windows still owns authentication, device drivers, hardware/policy restrictions, and any protected access prompt. A connection request is not reported as an observed connection; the device must be awake and its Windows profile/driver must support the requested use.

## Sound + Mixer

- Select playback and microphone devices; adjust each endpoint's level and mute state.
- Adjust individual running audio sessions in the application mixer. An app appears after it creates a Windows audio session.
- Clicking the speaker opens the sound tray panel: output/microphone selectors, separate percentage readings and Mute/Unmute buttons, and a link to the full mixer. It refreshes while visible and releases its polling timer when closed.
- Device dropdowns show friendly names. Switching a sound device preserves keyboard focus, and refreshes leave unchanged controls in place. Tab advances from a volume slider directly to its mute button.

## Display
 
- View each connected monitor's STRATA number, primary/extended role, physical resolution, current refresh rate, effective X/Y DPI, scale, and the combined virtual-desktop dimensions.
- Workspace and Center Stage animation cadence is selected from the monitor that owns the transition, rather than a single global/primary refresh value.
- Adjust display luminance slider when the display hardware exposes a supported software brightness channel.
- Drag monitors on the arrangement map or enter exact X/Y positions. Select resolution, refresh rate, orientation, primary display and enabled outputs. Extend and Duplicate use Windows display topology directly.
- Apply is a preview with **Keep** and **Restore**. A separate helper restores the previous configuration after 20 seconds or if the calling shell disconnects. Stale/hotplugged drafts are rejected. Changes become persistent only after Keep succeeds.
- Windows per-monitor DPI is displayed honestly as read-only; there is no supported general setter in this implementation. STRATA's interface scale remains separately adjustable under UI & Theme.
 
## Input + Keybinds

- The page offers separate **Open Keybinds** and **Open Keybind Editor** routes instead of an embedded shortcut dump.
- Adjust native pointer speed/precision, primary mouse button, wheel scrolling, double-click interval and keyboard repeat settings.
- Precision Touchpad controls appear when the OS and device expose the supported interface: scrolling, zoom, taps, pointer sensitivity and available haptic/force options. Device enable state is read-only. Older Windows builds and unsupported hardware display a capability explanation.
- Enable STRATA workspace swipes and set their threshold independently of the hardware settings.
- Open the complete searchable keybinding viewer (`Super + K`).
- Open the protected keybinding editor (`Super + Ctrl + K`) to remap or reset built-in actions and add, edit, or delete custom STRATA commands or application shortcuts.
- Use the per-row **Default** button to reset one built-in action without affecting other customizations.
- Select/search the action through the STRATA Command catalog, and type either `SUPER` or `WIN` for the Windows-logo key in edit mode.
- Reject duplicate or conflicting chords before saving; the editor and emergency recovery routes cannot be remapped.
- Confirm that the live list renders the Windows-logo icon where the chord uses `Super`.
- Review the non-`Super` emergency recovery chords.

Workspace Count accepts 1 through 10. `Super + -` decreases the same saved value only when the highest workspace is empty; `Super + +` increases it. The rail and an open Interface pane refresh from that setting.

## Windows Tweaks

- Hide native caption controls where the target window supports it.
- Apply the themed active-window border.
- Apply wallpaper-derived caption and title-text colors to compatible native DWM title bars; this reversible tweak is enabled by default.
- Synchronize Windows light/dark application preference.
- Synchronize each STRATA wallpaper change to the current user's Windows lock screen by default.
- Enable or disable future lock/sign-in wallpaper synchronization independently; disabling preserves the current lock-screen image.
- The protected sign-in background remains Windows-owned. The page explains this boundary alongside STRATA's lock-screen synchronization control.
- Theme Command Prompt and PowerShell palettes.
- Enable external theme adapters.
- Toggle application-window transparency support.
- Adjust **App Window Transparency** from `0%` (solid) through the `65%` readability limit for eligible third-party windows. `Super + Shift + T` retains the per-window toggle.
- Make STRATA Shell the default shell or restore Explorer through the edition-aware route: current-user policy on Windows 11 Home/Pro, Shell Launcher on supported Enterprise/Education/IoT editions.

Each compatibility tweak is saved independently and is designed to be reversible. Applications may ignore unsupported DWM or chrome operations.

## Startup + App Tray

- Browse/search the application launcher catalog and select apps to start with the installed STRATA shell. Selections use stable catalog identities; missing apps remain removable and cannot inject arbitrary commands. Preview and test modes do not launch them. These selections run at each shell startup, including a shell restart.
- Review startup registrations discovered from the supported per-user and machine startup locations.
- Use the top-rail tray dropdown for app-published controls such as Tailscale's own menu instead of adding one icon per application to the rail.
- Published tray entries retain their real application icon and expose separate Open and Menu actions.
- Open (or the application row) restores the most recently used tracked window across workspaces. When no window exists, it opens the publisher's native menu; choose the app's own Open action to create a window. Tray-only applications expose their controls there. Click-only publishers receive one activation fallback if no menu/window appears. Menu always requests the publisher's menu. No process is relaunched or terminated.
- Registrations are grouped into one base row per product/company; distinct publisher controls remain available from that row's submenu, and shell hardware pseudo-icons are excluded.
- Export or import a portable startup manifest. First run reconciles a supplied manifest once, preserves existing choices, rebases registered executable paths where possible, and skips missing/unsafe entries.
- An application that closes its visible window into the tray is removed from Center Stage without terminating its background process.
- Background applications without a published tray entry expose Show and a separately confirmed Exit action when STRATA Shell can identify their process.

## Power + Session

- Adjust separate plugged-in and battery screen-off/sleep timeouts for the active Windows power plan. Managed settings and unsupported options report their limitation.
- View battery charge, available capacity/health and charge/discharge rate. Configure the automatic Energy Saver battery threshold where supported. Instantaneous Energy Saver switching and historical per-app battery usage are not exposed by this implementation.
- Use the aligned two-column icon grid for clear session and power actions.
- Lock immediately.
- Sleep or hibernate.
- Sign out.
- Restart or shut down.
- Restart STRATA Shell or open a temporary Explorer Session when running as the shell. Explorer Session closes STRATA Shell and starts the normal Windows desktop for the current login without changing the configured default shell; STRATA Shell returns after sign-out or restart.
- Permanently restore Explorer only from **Windows Tweaks → Default Shell**. The setting explains that it changes future sign-ins, requests confirmation, and uses the edition-appropriate elevated Shell Launcher or per-user-policy recovery route.

Actions that end or replace the current session—including Explorer Session—require confirmation; lock does not. The emergency `Ctrl + Alt + Shift + Delete` recovery chord remains a permanent Explorer restore rather than a temporary session switch.

## Date + Time

- Select the Windows time zone and its daylight-saving behavior, request synchronization with the configured Windows time source, or set the local date and time. Invalid and ambiguous DST times are rejected. Privileged clock changes use a narrowly scoped helper and Windows elevation only after an explicit action.
- The bar clock opens the live clock/calendar; browse months or return to Today. Polling stops when the panel closes.

## Native settings boundaries

Routine settings no longer offer Start/Stop Explorer or external Windows Settings links. Windows Update offers an on-demand native availability check and history; update installation, pause/restart scheduling, protected sign-in policy, HDR/Night Light, language/IME and vendor-specific hardware controls remain outside this pass. This does not remove the deliberate temporary Explorer Session recovery action.

## Screensaver

- Enable or disable automatic idle activation.
- Choose an idle delay from 1 to 60 minutes; the shipped default is 5 minutes.
- Defer automatic activation during a visible fullscreen application, Windows presentation mode or full-screen Direct3D activity. The generic Windows "busy" notification state does not block an otherwise idle five-minute launch.
- Start the real full-screen surface from this pane or STRATA Command, provided no visible fullscreen application is active.
- Inspect the live STRATA state and read-only Windows timeout status. Windows sleep, display-off and lock choices take precedence.

The screensaver uses the active wallpaper theme—including a contrast-preserving light field and deep dark field—and the global High Contrast and Reduced Motion choices. Its shared uppercase STRATA vector mark, CRT scan treatment, heavy phosphor bloom, and adaptive particles stay crisp through 4K while 30 distinct effects rotate through smooth 1.6-second full-scene crossfades. Every monitor shares one animation timeline; the complete WPF vector scene is rebuilt at a stable maximum of 60 Hz while DWM presents at each display's native cadence. Any mouse movement, click, wheel action, touch/stylus input, or keyboard key dismisses it without leaving the global hotkey router's modifier state stuck.

## Updates

- Choose Stable for finished releases or Preview for the newest available release, including prereleases.
- Check the public binary-only STRATA Shell release feed without connecting a GitHub account.
- Check for updates without downloading anything, then use Update Now to download, validate, self-test, and install the selected immutable release.
- The running shell is not replaced in place. An activated installation begins using the new release at the next sign-in, preserving the existing Explorer recovery path.
- Update Control reports **Running Build** and **Installed Release** separately. When they differ, Settings states whether the installed release is ready for the next sign-in or is present but not active.
- Preview packages are lightweight and framework-dependent; they omit the .NET runtime, full Setup executable, wallpaper library, and Stable-only extras. Full self-contained packaging is reserved for an approved Stable promotion.
- The same page can query native Windows software-update availability and recent history without Explorer. Windows Update installation, pause and restart scheduling remain outside this implementation.

Downloaded archives must use the `StrataShell-*.zip` naming policy. Hardened clients require GitHub's SHA-256 digest and a release-bound manifest signed by the trusted STRATA update key. STRATA enforces compressed/expanded limits, rejects traversal paths, checks the complete payload and runs its self-test before installation. Windows publisher signing is separate and remains deferred.

## About

- View a theme-reactive STRATA logo and fetch-style system summary.
- Inspect host, Windows build, the running Build identifier, stable-only Engine version, theme, uptime, CPU, GPU, memory, displays, architecture, and runtime at a glance. Preview publication changes Build but retains the latest Stable Engine version.
- See whether the Explorer failsafe is installed and open project or recovery locations where available.

## Settings schema

`ShellSettings` has a versioned schema (`SchemaVersion 40`) and defaults. Schema 27 removes only overrides of the retired built-in panel/appearance chords while preserving user-created shortcuts that intentionally reuse those chords; it also normalizes missing custom-keybinding collections safely. Schema 28 adds global widget-title visibility and themed/normal weather-icon preferences, and removes stale overrides for replaced monitor and silent-workspace chords. Schema 29 selects normal weather icons as the default while retaining themed glyphs as an explicit choice. Schema 30 separates the global blur strength from independently persisted STRATA-surface and widget targets while preserving existing surface behavior. Schema 31 adds performance mode, wallpaper scaling, animation fps caps, and battery saver settings. Schema 32 originally added Windows background-process suppression; schema 38 retires that option and forces its legacy flag off. Schema 33 adds the wallpaper fit mode (`Fill`, `Fit`, `Stretch`) with aspect-ratio-aware decode resolution and DPI-aware monitor sizing. Schema 37 reduces the default Floating dock height from 50 to 48 DIPs while preserving customized heights. Schema 39 removes the CPU/power/Bluetooth bar flags and unused spring-motion, inactive-opacity and Do Not Disturb fields. Power is mandatory; retired Super + Escape built-in remaps are removed while other customizations are retained. Schema 40 adds launcher startup selections and configurable STRATA touchpad workspace swipes while preserving existing choices. Unknown or missing values fall back through the settings service rather than preventing shell startup. Local values include paths and machine preferences, so `settings.json` is intentionally ignored by Git.

## Phase 2 performance behavior (local candidate)

Automatic low effects now reads actual battery/energy-saver state and temporarily disables bloom and surface/widget frost without overwriting the saved values. Manual High overrides automatic low effects. Selected frame caps apply to dispatcher-driven workspace/slot transitions as well as worker-driven animation. Sustained missed budgets can lower subsequent optional animation cadence to 60 Hz outside High mode. See [current validation and limitations](STATUS.md).

## Phase 4 app controls

No shell settings-schema change. Text's encoding/newline button affects the current document on save. Browser's Site permissions control reviews the active profile and can reset an individual permission or all saved choices; reload open pages to stop existing access. Permission prompts distinguish one request from an explicitly remembered allow. Media honors Reduced Motion and uses the existing shared system-audio source. Terminal stores its isolated runtime data under `TerminalWebView` in the settings directory; Browser continues using `BrowserData` beside the settings files.
