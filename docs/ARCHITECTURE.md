# Architecture

STRATA Shell is a .NET 9 WPF process that replaces Explorer's visible desktop shell for one user while continuing to use Windows for DWM composition, application hosting, drivers, security surfaces, packaged-app activation, and hardware APIs.

## Process modes

`App.xaml.cs` selects one of four important roles:

1. **Preview:** composes the interactive shell without asserting installed-shell recovery behavior.
2. **Shell:** owns desktop surfaces, global bindings, window orchestration, and the Explorer-return exit code.
3. **Bootstrap:** launches and watches the real shell child. It prevents repeated login crash loops.
4. **Utility/test:** isolated STRATA Terminal elevation, self-test, critical UI, audio loopback, application activation, or Explorer restoration.

The edition-aware installer points either the current-user custom-shell policy (Home/Pro) or Windows Shell Launcher (supported Enterprise/Education/IoT editions) at `StrataShell.exe --bootstrap`, never at a mutable development build.

## Composition root

`ShellCoordinator` is the composition root. It owns the long-lived services and first-party windows, builds the authoritative keybinding list, routes launcher commands, and performs ordered shutdown.

Native settings use shared display, sound, input, power, time and wireless services. `DisplayConfigurationService` starts an authenticated, current-user named-pipe `--display-guard` helper before applying a display draft; it validates, previews, commits or rolls back independently of the WPF UI. `--clock-control` is restricted to explicit set/sync actions. These helper modes are dispatched before normal shell startup. Hardware mutations are excluded from isolated tests; fake controllers exercise confirmation and failure paths.

Browser, Files, Text, Paint, Images, Terminal and Media launches create separate document windows. Browser windows share one profile storage service per SettingsService to prevent stale in-memory bookmark/history/settings stores from overwriting one another. Dock groups retain individual window identities, normal close messages and per-monitor icon bounds. `DockPreviewWindow` registers bounded, disposable DWM thumbnail pages. Shell-hook minimize requests receive the visible dock icon rectangle; STRATA minimizes use a short compositor-thumbnail transition that leaves real restore bounds untouched. Reduced motion/low effects bypass custom animation, and fullscreen closes previews/transitions.

`WindowManagerService.DesktopMode` owns the shell-wide Tiled/Floating transition. Floating windows use the existing HWND registry, monitor/workspace ownership, chrome recovery and focus services. `DockService` groups windows by stable first-party, executable or packaged-app identity and persists pins through `SettingsService`. Each `TopRailWindow` reuses its native appbar reservation at the selected top/bottom edge. `RailPanelPlacement` supplies shared DIP geometry for the launcher and quick panels; `DesktopModePolicy` filters tiling-only shortcuts after customization. See [Floating mode](FLOATING_MODE.md).

Primary collaborators:

- `SettingsService` — versioned per-user settings and persistence.
- `ThemeService` — wallpaper selection, palette state, visual settings, and change notification.
- `WindowManagerService` — top-level window discovery, workspace state, focus, placement, opacity, chrome, and transitions.
- `SystemControlService` — audio, display, network, Bluetooth, media, terminal, browser, session, and power routes.
- `WindowsLockScreenService` — coalesced current-user lock-screen staging, format normalization, supported personalization activation, and bounded cleanup.
- `AppDiscoveryService` and `PackagedAppService` — merged application catalog and MSIX activation.
- `HotkeyService` — validated global keyboard policy and dispatch, including privilege-boundary state reset, hook reinstall, and elevated-process exit recovery.
- `IdleScreensaverService` — last-input monitoring, presentation deferral, per-monitor surface lifecycle, input consumption, and read-only Windows idle-policy inspection.
- `ShellRecoveryService` — installed-state records, watchdog contract, self-test, and Explorer-return signaling.

## First-party surfaces

| Surface | Responsibility |
|---|---|
| `DesktopWindow` | Wallpaper canvas, persistent widget columns, hover expansion, and live stage-envelope measurement |
| `TopRailWindow` | One reserved appbar edge per monitor, dedicated workspaces, active-app context, hardware modules, and launch/settings/power routes |
| `EdgeGlowWindow` | Four-sided surround and theme bloom |
| `LauncherWindow` | Command/application search and single-click/keyboard activation |
| `QuickPanelWindow` | Audio, network, Bluetooth, display, clock, system, and power edge panels |
| `SettingsWindow` | Organized settings pages, including the live screensaver state and compatibility routes |
| `ScreensaverWindow` | Opaque per-monitor host for the approved theme-derived STRATA logo renderer |
| `FileManagerWindow` | Explorer-independent file navigation and operations |
| `TerminalWindow` | Persistent CMD/PowerShell sessions, history, working directory, scripts, and captured output |
| `TextEditorWindow` | Encoding-aware plain-text editing with atomic save and STRATA Files routing |
| `ScreenshotWindow` | Multi-monitor rectangle/window/desktop capture, markup, clipboard, and export |
| `WallpaperChooserWindow` | Search, thumbnails, pagination, and preview |
| `KeybindWindow` | Searchable read-only `Super + K` policy plus protected `Super + Ctrl + K` remap/custom-shortcut editor mode |
| `OsdWindow` | Short-lived volume, wallpaper, workspace, and system signals |
| `PromptWindow` | Themed confirmations/input |

All first-party surfaces consume the same palette and visual settings. The wallpaper-frost cache supplies one monitor/strength texture to independently enabled first-party windows and desktop widget cards. `ThemeService` keeps the frost source on the currently presented wallpaper until `DesktopWindow` commits the incoming crossfade, then releases one synchronized material refresh. Shell chrome and widget hosts are excluded from managed application workspaces; document apps participate in workspace, focus and dock management.

## Center Stage policy

Window layout is policy-driven rather than a recursive tiling tree.

```text
workspace
├── top slot    (or the single full-height app)
└── bottom slot (present only when a second app exists)
```

- `CenterStageLayout` calculates one-app and two-app logical rectangles.
- `StageVerticalEnvelopePolicy` aligns stage geometry to the desktop's rendered widget-card envelope.
- `WidgetColumnLayout` keeps left/right columns and the center lane stable under UI scaling.
- `WindowFrameGeometry` compensates for invisible DWM frame extents so different toolkits have equal visible edges.
- `NewWindowWorkspacePolicy` chooses the current or next workspace with capacity.
- `CenterStageSwapTransition` animates top/bottom exchange.
- `WorkspaceSlideTransition` animates complete directional workspace travel.
- `WindowChromePolicy` applies best-effort caption/control changes.
- `WindowCompatibilityPolicy` keeps Microsoft Media Player on one primary opaque stage surface and rejects its owned/transient playback surfaces.

In Tiled mode, each workspace has a strict two-application capacity. Incoming movement to an occupied slot is an exchange. Floating mode allows ordinary overlapping windows and grouped dock access.

## Window discovery and focus

`WindowManagerService` observes eligible top-level windows, filters shell/owned/tool surfaces, assigns new windows to policy slots, applies visible-frame geometry, and focuses the selected HWND. Floating geometry survives leave/return workspace transitions. In the Tiled environment, per-window floating presentation uses separate normalization and capacity rules; it does not select the shell-wide Floating environment. The existing native move/resize routes preserve monitor and workspace ownership. The foreground floating window owns overlap precedence: covered peer windows are hidden and restored as geometry/focus changes, while the desktop consumes its bounds through `WidgetOcclusionPolicy`. The top rail independently consumes every visible workspace-window bound and ducks whenever one crosses the reserved edge. The active-window overlay owns its target HWND and synchronizes physical visible-frame geometry on WPF composition frames; `ShellCoordinator` suppresses that overlay for the complete screensaver lifecycle. Interrupted directional transitions commit their focus intent before the next held-key command resolves monitor ownership. Native-only motion retains the per-monitor high-resolution scheduler. Any transition containing a same-process STRATA WPF surface moves every native and managed frame as one synchronous deferred batch from `CompositionTarget.Rendering`; expensive first-party effects and frost alignment pause during geometry motion and resume after the exact final tile. This removes cross-thread/producer-queue lag without using queued `SWP_ASYNCWINDOWPOS`. Managed appearance is restored on shutdown through `WindowAppearanceRecoveryStore`.

`DisplayTopologyService` records the native display device name, current `DEVMODE` refresh rate, and effective per-monitor DPI alongside physical bounds. Workspace/slot animation workers use the transition monitor's rate. Screensaver windows share a high-resolution time origin while independently pacing WPF invalidation to their monitor, keeping mixed-refresh output phase-aligned. The saver keeps its ambient scene continuous and single-pass, uses a frozen tiled CRT layer, bounds bloom passes, and adjusts particle density against resolution and refresh so mixed-rate rendering does not multiply complete 4K scene work on the UI thread.

The service uses Win32/DWM APIs because WPF owns only STRATA's windows. It cannot force every unrelated toolkit to accept border, opacity, caption, focus, or geometry changes.

## Theme pipeline

```text
wallpaper file
    ↓
PaletteExtractor
    ↓
ThemePalette + light/dark decision
    ↓
ThemeService
    ├── WPF resources and first-party surfaces
    ├── active-window border and edge surround
    ├── widget charts, spectrum, vibrancy, frosted glass, and palette-wide chromatic bloom
    ├── WindowsThemePreferenceService
    └── ConsoleThemeService
```

The wallpaper library has independent Light and Dark folders plus a recursive debounced watcher. Changes invalidate the collection cache and refresh the chooser/cycling routes immediately. Auto mode derives the application-mode decision from image luminance. `WindowsThemePreferenceService` synchronizes the per-user Windows app preference; `ConsoleThemeService` updates Command Prompt and PowerShell color behavior.

`FrostedWallpaperBackdrop` renders the active wallpaper to a bounded per-monitor texture on a dedicated STA worker, applies a continuous Gaussian radius, freezes and caches the result, then exposes the correct screen-space crop beneath each first-party window. The ordinary `SurfaceBrush` remains the single tint/transparency owner above that layer. This avoids fixed-opacity system Acrylic bands, repeated full-resolution blur work, and a Windows App SDK runtime requirement.

## Widgets and media

- `WeatherService` fetches and normalizes weather data.
- `AudioSpectrumService` captures the active system output mix using WASAPI and publishes FFT bands in memory.
- `AiCliService` runs the selected local CLI with streamed output and cancellation.
- The YouTube widget uses a local `assets/youtube/player.html` host inside WebView2. Local hosting provides stable origin/embedding behavior and keeps playback alive while the visual card changes size.
- `WidgetOcclusionPolicy` fades widgets when menus or managed windows need the same screen area.

External accounts, AI CLIs, and YouTube content remain owned by their respective providers; STRATA Shell does not bundle credentials.

## Lock-screen synchronization

`ThemeService` queues `WindowsLockScreenService` after initialization, explicit wallpaper selection, collection cycling, and watched-file refresh. Synchronization never blocks the shell's wallpaper transition. Each request stages a uniquely named local JPEG or PNG before calling the current-user Windows personalization API; pending older requests are discarded, and successful updates remove obsolete staged copies. The feature is enabled by schema default and migration but remains independently switchable under Windows Tweaks.

The service sets the ordinary current-user lock screen, not a machine policy. Windows owns whether that same picture appears on the protected sign-in surface; STRATA explains that boundary without modifying secure logon policy.

## Files architecture

`KnownFolderService` resolves Windows Known Folders instead of concatenating user-profile paths. `FileManagerWindow` owns cancellable background enumeration, debounced filtering, Explorer-style buffered type selection, persistent Details/Icon presentation, theme-colored selection, selection, history, watcher updates, clipboard operations, drag/drop, hidden filtering, properties, and error display. `StrataFilePickerService` hosts that same complete surface in Open, Save, or Select Folder mode, preserving file filters, default extensions, overwrite confirmation, and owner-centered modality without a Windows Explorer dialog. It is the required route for first-party wallpaper, text, screenshot, terminal, startup-manifest, AI working-directory, and import/export workflows. `RecycleBinService` safely enumerates the current user's local-drive recycle metadata, restores entries to their original locations, and supports confirmed permanent deletion and emptying without relying on Explorer's namespace. `VirtualizingWrapPanel` realizes only visible icon tiles. Thumbnail work prioritizes realized, selected, and recently changed files, publishes each preview independently, directly decodes common image formats, and falls back to registered Windows providers for video, PDF, and modern codecs. `FileItem` is the view model, preventing the object-type text rendering bug that occurs when a list binds directly to an untemplated object.

## Hardware and secure routes

`SystemControlService` uses Core Audio for endpoint selection, the native WLAN API for Wi-Fi discovery and profiles, and `Windows.Devices.Enumeration`/Bluetooth APIs for device discovery, pairing, connection requests, forced radio disconnect, and removal. The rail panel and Settings call the same service, so they expose the same state and controls. Wi-Fi passwords are accepted through a themed secret prompt and enrolled directly in memory with WlanSetProfile in the current user profile store. No temporary credential document is created. Adapter GUID, exact profile name, SSID bytes, authentication and cipher remain distinct. Unknown security never falls back to open; hidden/enterprise and unsupported enrollment types require an existing Windows profile. Requests run off the dispatcher, connections are serialized and cancellable, and denial/service errors have explicit status.

`NotificationAreaService` owns Explorer-independent notification-area registrations. It answers `ABM_GETTASKBARPOS` through ManagedShell's shared-memory transport before `TaskbarCreated`, allowing clients such as Tailscale to detect the tray host before registering. The primary rail supplies physical top/bottom host bounds; initial icon rectangles remain valid before dropdown layout. It resolves Known Folder registrations, filters Windows hardware carrier processes, groups rows by product/company identity, preserves distinct publisher controls beneath the base application, and forwards native context-menu/activation messages. Open first restores a tracked window belonging to the publisher process or exact executable path, then falls back to its own menu when no window exists. The top rail renders only one down-chevron/count entry; individual app icons and actions remain inside its dropdown. `WindowManagerService` distinguishes a window hidden by its own application from one hidden by STRATA Shell, allowing close-to-tray applications to keep running without occupying a workspace slot.

`TransientWindowPlacement` centers shell-owned modal surfaces in the work area of their reference window's monitor. This prevents narrow top-rail and side-panel owners from pulling wallpaper selection or confirmation dialogs away from the visible center while preserving modal ownership and z-order. `StrataDialogWindow` explicitly reasserts topmost foreground placement after centering; diagnostic dialogs may expose **Copy Error** so a shell-owned failure cannot be hidden behind the desktop or lost when dismissed.

STRATA Shell initiates ordinary hardware workflows without opening Windows Settings. Windows remains responsible for Bluetooth authentication; its supported custom-pairing API delegates confirmation-only, display-PIN, provide-PIN and confirm-match interactions to STRATA. Pairing uses AssociationEndpoint identities and the default Windows protection level. UAC, sign-in, protected access prompts and drivers remain Windows-owned.

`WirelessPanel` is shared by Settings and the network/Bluetooth rail menus, through `SystemControlService`. Bluetooth uses a reference-counted `BluetoothDiscoveryService` with Classic/LE AEP watchers, coalesced Added/Updated/Removed notifications and bounded device storage. Enumeration does not create connections to every paired device. Closing the last view stops its watchers. Rows use physical addresses or explicit endpoint identities, never friendly names as identity. Pairing deferrals are completed on acceptance, rejection and exceptions, with transient PIN display cleared when the operation ends.

Wi-Fi radio state uses native WLAN PHY queries/setters; hardware blocks are reported rather than bypassed. Profile deletion targets an exact adapter GUID and case-sensitive name, with managed profiles protected. Profile metadata never requests plaintext keys. Native scans show cached results immediately and refresh them over a bounded window; closing the surface cancels pending refreshes. The flyout is shown before asynchronous hardware discovery, and modal pairing/password prompts retain their owner until the interaction completes.

## Split-repository update architecture

Private source lives in `ManiaxMax/StrataShell`; compiled distribution assets live in the public, binary-only `ManiaxMax/StrataShell-Releases` repository. `GitHubUpdateService` reads that public release feed without requiring sign-in. Stable filters out prereleases; Preview accepts the newest non-draft release.

Release assets must match `StrataShell-*.zip` and stay within compressed/expanded inventory limits. Each update uses a unique attempt directory beneath local STRATA state, requires the GitHub-provided SHA-256 digest and verifies a release-bound signed manifest against the embedded STRATA public key before extraction or execution, rejects archive traversal, requires the known `payload` and `scripts/Install-StrataShell.ps1` structure, and invokes the existing immutable installer in a hidden process. Activated shells switch on the next sign-in rather than replacing the running executable. In `--shell` mode, current-release detection prefers the installed directory actually hosting the running shell over stale recovery metadata and preserves a valid newer pending release. Identical same-tag payloads are reused; mismatched or partial locked directories remain untouched while the installer chooses a deterministic content-fingerprinted physical directory and records the canonical tag in `.strata-release.json`. An installed Preview-mode process cannot rewrite the active installation record. Full failures are logged and persisted separately from the modal presentation.

For the default owner testing path, the verified private source commit is pushed to the source repository's `preview` branch before `scripts/Publish-StrataPreviewRelease.ps1` publishes a matching framework-dependent ZIP as a prerelease targeting the distribution repository's `preview` branch. Preview omits the .NET runtime, full Setup executable, wallpaper library, and Stable-only extras. The package carries only self-contained host metadata plus a hash inventory; if no registered runtime is available, the installer may hard-link the exact compatible runtime files already installed by Stable. Publication verifies that bridge with global runtime lookup disabled and rejects any runtime binary in the ZIP. An approved `main` promotion uses the full self-contained installer pipeline.

## Recovery architecture

```text
Windows 11 Home / Pro                 Enterprise / Education / IoT
current-user Shell policy             Windows Shell Launcher
              └──────────────┬──────────────┘
                     StrataShell.exe --bootstrap
                                  ↓
                            STRATA --shell
```

- Edition selection is automatic. The current-user route is non-elevated; the Shell Launcher route uses the elevated `Configure-IotShellLauncher.ps1` helper through `WESL_UserSetting` and retains Explorer as its default fallback.
- A local marker distinguishes intended shell mode from ordinary preview.
- Three unexpected exits within ten minutes, including exit code zero, cause bootstrap to restore Explorer. Explicit restart (44) and Explorer exits (42/43) are excluded. A per-child named-event heartbeat runs on the shell dispatcher; bootstrap offers a separate recovery window after a sustained 30-second stall, with 60-second startup/resume grace and lock/suspend deferral. It never kills the shell solely because a heartbeat is late. Only explicit recovery stops that shell process; external application processes are preserved.
- Recovery scripts live in `%USERPROFILE%\Strata Recovery`, outside versioned releases.
- A non-`Super` recovery chord signals the bootstrap and returns Explorer.
- The previous current-user policy value is backed up once and restored exactly; recovery invokes Shell Launcher cleanup only for installations that used it.

See [Installation and recovery](INSTALLATION.md) for operational details.

## Trust boundaries

- STRATA Shell owns its WPF windows, settings, layout policy, and local child processes.
- Windows owns DWM, secure desktop, Shell Launcher, drivers, protected device trust ceremonies, and session primitives.
- Third-party applications own their contents, custom chrome, protected media, and accessibility behavior.
- WebView2/YouTube and local AI CLIs are optional integrations with separate security and availability boundaries.

This separation is intentional: replacing Explorer's experience must not weaken Windows security or pretend STRATA Shell can safely reimplement protected surfaces.

## Batch 1 trust and file boundaries

BrowserMessagePolicy limits messages to token-bound start-page navigation/search; extension installation is native-only. CrxPackageVerifier verifies identity before bounded staging/consent. BrowserPasswordStore uses DPAPI CurrentUser with atomic migration and fail-preserved storage; BrowserCredentialConsent gates reveal/copy through Windows Hello.

SafeArchivePath, AtomicFile, FileTransferRequest and FileTransferSafety share file/archive safety. DisplaySurfaceLifetime detaches the old primary rail before topology replacement. See [current validation and limitations](STATUS.md) and [signing lifecycle](RELEASE_SIGNING.md).

## Phase 2 responsiveness boundaries

SystemTelemetry publishes immutable observations from separate single-flight background lanes; recurring rail/widget refreshes do not enumerate processes or audio devices. SharedAudioSpectrum owns one capture/FFT per source and desktops subscribe only while their visual is active. Wallpaper and frost caches are bounded by pixel bytes, with a common two-worker decode limit and a dedicated STA for frost composition. Paint stores bounded XOR tile deltas and guards document edits during background import/fill/save operations.

Browser WebView2 objects remain on their owning UI thread. Window-creating/closing actions are deferred out of callbacks; background tabs request Low memory without suspending connections. The browser root no longer carries a WPF shader above native web content. DwmFlush runs through a single-flight worker after border retirement. Periodic full window reconciliation yields during active drags. Dispatcher monitoring and bounded logs provide stall evidence without adding a blocking graphics wait. See [current validation and limitations](STATUS.md).

## Phase 3 display reconciliation and fullscreen

Display topology is keyed by Windows monitor device-interface identity rather than transient HMONITOR values. Reconciliation is debounced after topology/resume/session events, cancels active placement transactions, maps surviving identities, clamps migrated floating/restore geometry and moves disconnected-display apps onto surviving workspaces. If more than twenty apps land on one monitor, STRATA switches to its existing Floating mode to preserve access. Per-display preferences are capped at sixteen entries and saved without raw device IDs.

The primary widget host follows the primary display without recreation. Secondary widget hosts are reused by identity and retained hidden while disconnected so timers, AI sessions and media intent survive. Gesture input rebinds to the replacement rail; widget occlusion and stage envelopes are per display. Retained secondary sessions last until reconnection or shell shutdown and can retain resources.

Native fullscreen windows bypass tiling, opening fades, material transparency and background dimming. Any preexisting constant-alpha layer is held opaque while fullscreen and restored afterward. Fullscreen monitor assignment follows native geometry, and spanning windows suppress every covered display. Per-display surround, rail/dock and widget visuals are suppressed; launcher, quick panels, app switcher and ordinary OSDs are gated. Delayed panel builds cannot reopen over fullscreen. Minimized games no longer get forcibly restored by the tiler. External foreground fullscreen can suppress chrome even when the window cannot be adopted. Windows secure UI and explicitly requested emergency recovery remain available.

## Phase 4 app lifetimes and rendering

`TerminalSession` uses ConPTY for interactive windows. `PseudoConsoleSession` drains output and writes input on separate workers, serializes startup/teardown, and closes the pseudoconsole away from WPF. `TerminalSurface` serves embedded xterm.js and Fit addon bytes at an intercepted reserved `.invalid` origin; every other request/navigation is rejected. No host objects, extension context, clipboard OSC bridge or external assets are enabled. A bounded output queue, one acknowledged 32K-character render batch and an event-started pump prevent unbounded producer/UI queues; UTF-16 surrogate boundaries survive serialization. Legacy fallback output is separately bounded.

Text uses load/edit/document generations and atomic snapshot saves with a SHA-256 conflict check. Snip freezes a drawing snapshot on WPF and composes/encodes in workers. `ImageOrientation` supplies exact EXIF and export transforms. Media subscribes to `SharedAudioSpectrum` only while its visualizer needs real system-output data. File copying uses native `CopyFileExW` into a temporary file to support progress/cancellation without dropping alternate streams. Copy/paste/drop and extraction share cancellation UI.

`AppSurfacePolish` attaches one WPF Loaded class handler to convert known local palette-brush references into dynamic resources and label tooltip-bearing buttons. `WindowManagerService` excludes foreground-owned/covered windows from border selection and suppresses delayed border presentation while a modal surface covers its owner. Task actions hold a process handle and compare creation time, so a recycled PID is never the intentional target.

Explicit monitor/workspace selection retires the previous pending focus request, including when the destination is empty. Deferred foreground notifications honor the existing manual monitor-selection lease and presentation transaction. Pointer selection and Alt+Tab release that lease; ordinary automatic Windows foreground fallback cannot redirect repeated workspace commands. Ctrl+Left/Right and Super+Tab share this routing.

Task Manager maintains one worker-owned PDH query, with bounded wildcard buffers, native language-neutral counter registration and delayed retry of unavailable providers. DXGI adapter LUIDs associate GPU engines and memory with their adapter; parallel engines are never added into a fictitious GPU percentage. Query disposal runs off WPF. Metadata refreshes at most every thirty seconds; there is no added shell-wide telemetry timer.

The same worker reads NVIDIA temperatures from System32 NVML and optionally CPU sensors from an already-running Libre/Open Hardware Monitor WMI namespace. Provider discovery backs off for thirty seconds; CPU sampling is bounded and stale values clear on provider failure. No elevation or sensor-driver installation occurs. Network sampling excludes addressless WFP/QoS filter layers to avoid repeating their parent interface's traffic.

Normal maximization in Floating view is enforced for external and STRATA-owned windows. In replacement-shell mode, `ShellWorkAreaReservation` publishes each monitor's native work area with `SPI_SETWORKAREA`; `SHAppBarMessage` alone depends on Explorer. Changes are session-only, use physical monitor coordinates and restore their previous value only while STRATA still owns it. Preview alongside Explorer uses normal appbar registration. Fullscreen and shutdown release the reservation. The platform contract is documented by [Microsoft](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-systemparametersinfow).

For desktop toolkits that request full-monitor bounds anyway, `WindowManagerService.MaximizedWorkArea` retains visible-frame correction during existing foreground/geometry reconciliation. It preserves maximized state and normal restore geometry. Three consecutive rejected corrections stop further attempts; reaching the correct bounds, restoring or changing the target resets that budget. A normal maximized custom-chrome window is excluded from game-fullscreen classification. Reservation sizes remain in physical pixels without a 120-pixel cap, so higher DPI cannot truncate the reserved bar height.

## September 2026 cleanup

Native opening/closing fades use `RenderFrameAnimation`: WPF composition callbacks, the monitor's effective frame-rate policy, and one completion deadline for non-presenting desktops. Cancellation removes rendering callbacks. `SurfaceAnimator` versions each operation and restores pre-reveal transforms; interrupted fades cannot fire stale completion actions.

Widget columns keep usable expanded card heights inside scrollable viewports. Reparenting invalidates pending hover work and clears the old column owner before restoring explicit locks. Scrolling refreshes hosted-browser input geometry; drag-edge scrolling stops on cancellation and shutdown.

The dock's Applications event is separate from the tiled Command launcher event. Power visibility is mandatory. Tray rows bubble left-click input so embedded action buttons retain their clicks. Placement and publisher foreground permission are recorded before dismissing STRATA's panel; native callbacks run after WPF popup capture is released. Open restores a tracked window or requests its native menu; Menu always requests the menu. If no interactive surface appears after three 100 ms observations, Open delivers one default activation (single selection for modern publishers, legacy double-click otherwise). Native menu-mode flags and visible publisher popup windows prevent a second activation from cancelling Tailscale-style menus. New input, another panel action, or a vanished publisher cancels the pending fallback. There is no process restart or force-kill fallback.

Explicit tray actions send deterministic single/double/right-click sequences with version-correct callback parameters. They do not infer gestures from the dependency's click timestamps. Native outline windows are owned by their target application and use its normal z-order band; the window manager additionally suppresses outlines covered by higher visible windows.
