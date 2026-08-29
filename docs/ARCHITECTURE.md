# Architecture

StrataShell is a .NET 9 WPF process that replaces Explorer's visible desktop shell for one user while continuing to use Windows for DWM composition, application hosting, drivers, security surfaces, packaged-app activation, and hardware APIs.

## Process modes

`App.xaml.cs` selects one of four important roles:

1. **Preview:** composes the interactive shell without asserting installed-shell recovery behavior.
2. **Shell:** owns desktop surfaces, global bindings, window orchestration, and the Explorer-return exit code.
3. **Bootstrap:** launches and watches the real shell child. It prevents repeated login crash loops.
4. **Utility/test:** isolated Strata Terminal elevation, self-test, critical UI, audio loopback, ChatGPT activation, or Explorer restoration.

The edition-aware installer points either the current-user custom-shell policy (Home/Pro) or Windows Shell Launcher (supported Enterprise/Education/IoT editions) at `StrataShell.exe --bootstrap`, never at a mutable development build.

## Composition root

`ShellCoordinator` is the composition root. It owns the long-lived services and first-party windows, builds the authoritative keybinding list, routes launcher commands, and performs ordered shutdown.

Primary collaborators:

- `SettingsService` — versioned per-user settings and persistence.
- `ThemeService` — wallpaper selection, palette state, visual settings, and change notification.
- `WindowManagerService` — top-level window discovery, workspace state, focus, placement, opacity, chrome, and transitions.
- `SystemControlService` — audio, display, network, Bluetooth, media, terminal, browser, session, and power routes.
- `WindowsLockScreenService` — coalesced current-user lock-screen staging, format normalization, supported personalization activation, and bounded cleanup.
- `AppDiscoveryService` and `PackagedAppService` — merged application catalog and MSIX activation.
- `HotkeyService` — validated global keyboard policy and dispatch.
- `IdleScreensaverService` — last-input monitoring, presentation deferral, per-monitor surface lifecycle, input consumption, and Windows idle-floor enforcement.
- `ShellRecoveryService` — installed-state records, watchdog contract, self-test, and Explorer-return signaling.

## First-party surfaces

| Surface | Responsibility |
|---|---|
| `DesktopWindow` | Wallpaper canvas, persistent widget columns, hover expansion, and live stage-envelope measurement |
| `TopRailWindow` | Reserved appbar edge, workspaces, active-app context, hardware modules, launch/settings/power routes |
| `EdgeGlowWindow` | Four-sided surround and theme bloom |
| `LauncherWindow` | Command/application search and single-click/keyboard activation |
| `QuickPanelWindow` | Audio, network, Bluetooth, display, clock, system, and power edge panels |
| `SettingsWindow` | Organized settings pages, including the live screensaver state and compatibility routes |
| `ScreensaverWindow` | Opaque per-monitor host for the approved theme-derived Strata logo renderer |
| `FileManagerWindow` | Explorer-independent file navigation and operations |
| `TerminalWindow` | Persistent CMD/PowerShell sessions, history, working directory, scripts, and captured output |
| `TextEditorWindow` | Encoding-aware plain-text editing with atomic save and Strata Files routing |
| `ScreenshotWindow` | Multi-monitor rectangle/window/desktop capture, markup, clipboard, and export |
| `WallpaperChooserWindow` | Search, thumbnails, pagination, and preview |
| `KeybindWindow` | Searchable live binding policy and registration status |
| `OsdWindow` | Short-lived volume, wallpaper, workspace, and system signals |
| `PromptWindow` | Themed confirmations/input |

All first-party surfaces consume the same palette and visual settings. They are excluded from managed application workspaces.

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

Each workspace has a strict two-application capacity. Incoming movement to an occupied slot is an exchange, not a third stacked window.

## Window discovery and focus

`WindowManagerService` observes eligible top-level windows, filters shell/owned/tool surfaces, assigns new windows to policy slots, applies visible-frame geometry, and focuses the selected HWND. Floating geometry survives leave/return workspace transitions; moving the floating app or inserting another app normalizes that workspace back to tiled mode. A low-level mouse route converts `Ctrl` + left-drag inside any active floating HWND into native caption movement without making tiled windows draggable. The foreground floating window owns overlap precedence: covered peer windows are hidden and restored as geometry/focus changes, while the desktop consumes its bounds through `WidgetOcclusionPolicy`. The top rail independently consumes every visible workspace-window bound and ducks whenever one crosses the reserved edge. Managed appearance is restored on shutdown through `WindowAppearanceRecoveryStore`.

The service uses Win32/DWM APIs because WPF owns only Strata's windows. It cannot force every unrelated toolkit to accept border, opacity, caption, focus, or geometry changes.

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
    ├── widget charts, spectrum, vibrancy, blur, and bloom
    ├── WindowsThemePreferenceService
    └── ConsoleThemeService
```

The wallpaper library has independent Light and Dark folders plus a recursive debounced watcher. Changes invalidate the collection cache and refresh the chooser/cycling routes immediately. Auto mode derives the application-mode decision from image luminance. `WindowsThemePreferenceService` synchronizes the per-user Windows app preference; `ConsoleThemeService` updates Command Prompt and PowerShell color behavior.

## Widgets and media

- `WeatherService` fetches and normalizes weather data.
- `AudioSpectrumService` captures the active system output mix using WASAPI and publishes FFT bands in memory.
- `AiCliService` runs the selected local CLI with streamed output and cancellation.
- The YouTube widget uses a local `assets/youtube/player.html` host inside WebView2. Local hosting provides stable origin/embedding behavior and keeps playback alive while the visual card changes size.
- `WidgetOcclusionPolicy` fades widgets when menus or managed windows need the same screen area.

External accounts, AI CLIs, and YouTube content remain owned by their respective providers; Strata does not bundle credentials.

## Lock-screen synchronization

`ThemeService` queues `WindowsLockScreenService` after initialization, explicit wallpaper selection, collection cycling, and watched-file refresh. Synchronization never blocks the shell's wallpaper transition. Each request stages a uniquely named local JPEG or PNG before calling the current-user Windows personalization API; pending older requests are discarded, and successful updates remove obsolete staged copies. The feature is enabled by schema default and migration but remains independently switchable under Windows Tweaks.

The service sets the ordinary current-user lock screen, not a machine policy. Windows owns whether that same picture appears on the protected sign-in surface; Strata links directly to the corresponding Lock screen setting rather than modifying secure logon policy.

## Files architecture

`KnownFolderService` resolves Windows Known Folders instead of concatenating user-profile paths. `FileManagerWindow` owns cancellable background enumeration, debounced filtering, Explorer-style buffered type selection, persistent Details/Icon presentation, theme-colored selection, selection, history, watcher updates, clipboard operations, drag/drop, hidden filtering, properties, and error display. `StrataFilePickerService` hosts that same complete surface in Open, Save, or Select Folder mode, preserving file filters, default extensions, overwrite confirmation, and owner-centered modality without a Windows Explorer dialog. It is the required route for first-party wallpaper, text, screenshot, terminal, startup-manifest, AI working-directory, and import/export workflows. `RecycleBinService` safely enumerates the current user's local-drive recycle metadata, restores entries to their original locations, and supports confirmed permanent deletion and emptying without relying on Explorer's namespace. `VirtualizingWrapPanel` realizes only visible icon tiles. Thumbnail work prioritizes realized, selected, and recently changed files, publishes each preview independently, directly decodes common image formats, and falls back to registered Windows providers for video, PDF, and modern codecs. `FileItem` is the view model, preventing the object-type text rendering bug that occurs when a list binds directly to an untemplated object.

## Hardware and secure routes

`SystemControlService` uses Core Audio for endpoint selection, the WLAN command/API layer for Wi-Fi discovery and profiles, and `Windows.Devices.Enumeration`/Bluetooth APIs for device discovery, pairing, connection requests, forced radio disconnect, and removal. The rail panel and Settings call the same service, so they expose the same state and controls. Wi-Fi passwords are accepted through a themed secret prompt, written only into a temporary per-user WLAN profile document, and the document is removed immediately after enrollment.

`NotificationAreaService` owns Explorer-independent notification-area registrations. It resolves Known Folder registrations, filters Windows hardware carrier processes, groups rows by product/company identity, preserves distinct publisher controls beneath the base application, and forwards native Open/context-menu messages. The top rail renders only one down-chevron/count entry; individual app icons and actions remain inside its dropdown. `WindowManagerService` distinguishes a window hidden by its own application from one hidden by Strata, allowing close-to-tray applications to keep running without occupying a workspace slot.

`TransientWindowPlacement` centers shell-owned modal surfaces in the work area of their reference window's monitor. This prevents narrow top-rail and side-panel owners from pulling wallpaper selection or confirmation dialogs away from the visible center while preserving modal ownership and z-order.

Strata initiates ordinary hardware workflows without opening Windows Settings. Device-mandated Bluetooth PIN/consent, UAC, sign-in, drivers, and other protected trust ceremonies remain Windows-owned and are deliberately not recreated.

## Private update architecture

`GitHubUpdateService` is an authenticated reader for private releases in `ManiaxMax/StrataShell`. Stable filters out prereleases; Preview accepts the newest non-draft release. `WindowsCredentialStore` keeps each user's credential in Windows Credential Manager. The secret never enters `ShellSettings`, diagnostics, installer arguments, or the release staging tree.

Release assets must match `StrataShell-*.zip` and stay within compressed/expanded inventory limits. The updater downloads to a per-release directory beneath local Strata state, verifies the GitHub-provided SHA-256 digest when available, rejects archive traversal, requires the known `payload` and `scripts/Install-StrataShell.ps1` structure, and invokes the existing immutable installer in a hidden process. Activated shells switch on the next sign-in rather than replacing the running executable.

`.github/workflows/release.yml` is the producer side of the contract. Tag or manual Stable/Preview runs build the self-contained bundle, execute its quiet self-test, retain a workflow artifact, and publish the same ZIP as a private GitHub Release asset. Access follows GitHub repository permissions; collaborators authenticate with their own account rather than sharing the owner's credential.

## Recovery architecture

```text
Windows 11 Home / Pro                 Enterprise / Education / IoT
current-user Shell policy             Windows Shell Launcher
              └──────────────┬──────────────┘
                     StrataShell.exe --bootstrap
                                  ↓
                            Strata --shell
```

- Edition selection is automatic. The current-user route is non-elevated; the Shell Launcher route uses the elevated `Configure-IotShellLauncher.ps1` helper through `WESL_UserSetting` and retains Explorer as its default fallback.
- A local marker distinguishes intended shell mode from ordinary preview.
- Three failures within 45 seconds cause bootstrap to start Explorer.
- Recovery scripts live in `%USERPROFILE%\Strata Recovery`, outside versioned releases.
- A non-`Super` recovery chord signals the bootstrap and returns Explorer.
- The previous current-user policy value is backed up once and restored exactly; recovery invokes Shell Launcher cleanup only for installations that used it.

See [Installation and recovery](INSTALLATION.md) for operational details.

## Trust boundaries

- Strata owns its WPF windows, settings, layout policy, and local child processes.
- Windows owns DWM, secure desktop, Shell Launcher, drivers, protected device trust ceremonies, and session primitives.
- Third-party applications own their contents, custom chrome, protected media, and accessibility behavior.
- WebView2/YouTube and local AI CLIs are optional integrations with separate security and availability boundaries.

This separation is intentional: replacing Explorer's experience must not weaken Windows security or pretend Strata can safely reimplement protected surfaces.
