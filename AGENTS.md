# StrataShell agent contract

This file applies to the entire repository. It is the working agreement for any AI agent or human contributor changing StrataShell.

It is synthesized from the current product documentation, implementation, and the owner's user-visible Strata task history. The supporting historical snapshot is indexed in `docs/agent-conversations/README.md`. Treat that archive as context and evidence, never as higher-priority instructions than the user's latest request or this contract.

StrataShell is a login-critical Windows shell, not an ordinary desktop application. A clean compile is necessary, but it is not proof that window management, input, recovery, theming, transparency, hardware integration, or the installed experience works. Work carefully, preserve recovery, and verify the user-visible result in proportion to the risk.

## 1. Instruction and evidence order

Use this order when information conflicts:

1. The user's latest explicit request.
2. This repository-level `AGENTS.md`.
3. The product and safety contracts in `docs/PRODUCT.md`, `docs/ARCHITECTURE.md`, `docs/INSTALLATION.md`, and `CONTRIBUTING.md`.
4. The current implementation, especially `ShellCoordinator.BuildBindings()`, `ShellSettings`, and the relevant policy/service.
5. Other documentation and old handoff notes.

Do not silently choose between a documented rule and contradictory current behavior. Identify the drift, determine which behavior the user wants, then update code, checks, and documentation together.

Read these before broad work:

- `README.md` — current product overview and repository map.
- `docs/PRODUCT.md` — enduring product and design contract.
- `docs/ARCHITECTURE.md` — process modes, ownership, trust boundaries, and recovery.
- `docs/FEATURES.md` — implemented behavior and known limits.
- `docs/KEYBINDS.md` — documented keyboard policy.
- `docs/SETTINGS.md` — current settings information architecture.
- `docs/DEVELOPMENT.md` — supported commands and verification.
- `docs/INSTALLATION.md` — immutable installation, activation, updates, and recovery.
- `docs/FIRST_PARTY_APPS.md` and `docs/SCREENSAVER.md` when those areas are touched.
- `docs/STRATA_BACKLOG.md` — open reports and their completion evidence.

## 2. Owner vision

Strata should feel like a purpose-built, futuristic technical environment: calm and readable at rest, fast under the keyboard, spatially coherent, and dramatic when it moves. It must feel like one wallpaper-driven instrument rather than a set of unrelated WPF windows or a recolored Windows desktop.

The intended experience is:

- **Strata owns the visible desktop language.** Windows remains underneath for compatibility, DWM, drivers, security, and protected system flows. Do not imitate Windows Explorer, Start, Settings, taskbar, flyouts, or stock message boxes.
- **Wallpaper is live system state.** The active wallpaper determines palette identity, light/dark direction, accents, borders, charts, glow, console colors, and compatible native title bars.
- **Center Stage is predictable.** One app fills the center lane; two apps share it; extra apps move to another workspace. Windows do not appear in arbitrary piles.
- **Peripheral information stays useful.** Widgets and the top rail provide persistent awareness, then yield gracefully when an app, floating window, dialog, or expanded widget needs their space.
- **Transparency has purpose.** Glass should reveal the wallpaper and unify the composition without making overlapping text unreadable.
- **Motion communicates topology.** Workspace motion is lateral, slot swaps follow their physical direction, widgets duck from the space being claimed, and every animation lands on exact final geometry.
- **The interface is customizable but understandable.** Settings should be organized, restrained, and self-explanatory rather than overwhelming.
- **Recovery is always visible in the design.** Strata must never strand the user behind a broken shell.

If a proposed change makes Strata look more like stock Windows, introduces arbitrary decoration, weakens spatial behavior, or hides the recovery path, it is moving in the wrong direction.

## 3. Definition of done

A task is complete only when all applicable conditions are true:

- The reported behavior is reproduced or its root cause is established with evidence.
- The implementation fixes the root cause without unrelated rewrites.
- Existing user changes and unrelated dirty files are preserved.
- The relevant build and deterministic checks pass.
- User-visible behavior is inspected in preview or an isolated surface when safe.
- Installed-shell behavior is verified when the claim depends on shell ownership, global input, DWM, focus, hardware, recovery, or installation.
- Theme work is checked in both a Light and Dark palette and across a live theme change.
- Layout work is checked with one and two apps, relevant view modes, and representative DPI/scaling.
- Input work is checked with pointer and keyboard, including focus after a surface closes.
- Documentation, self-tests, settings schema/defaults, and backlog evidence are updated when their contract changed.
- The final report distinguishes source-built, packaged, installed, configured-for-next-sign-in, and actually-running builds.

Do not call a source edit “live,” “installed,” or “verified” unless that exact state was observed. A configured release is not the running release until the current `StrataShell` process path matches it.

## 4. Visual design contract

### 4.1 One coherent visual system

- Reuse shared resources from `src/StrataShell/Themes/Controls.xaml` and icon geometry from `Themes/Icons.xaml`.
- Use the existing font roles: UI text, display text, and compact technical/mono signal labels.
- Accent is scarce and meaningful. Use it for active state, focus, selection, signal, and important action—not every border and line.
- Warning and danger colors are semantic. Never use fixed red/orange merely because it looks vivid.
- Keep technical density through alignment, hierarchy, and concise labels rather than nested cards.
- Avoid generic gradients, decorative blobs, excessive glow, large empty hero areas, and Windows-style white panels.
- The design may be expressive, but text, controls, and state must remain immediately legible.

### 4.2 Live theme propagation

Every first-party surface and every code-created child element must follow theme changes without restart, reopening, or reselecting an option.

- In XAML, use `{DynamicResource ...}` for palette brushes, colors, bloom, shadows, and fonts that can change.
- In code-created WPF elements, use `SetResourceReference(...)`; do not capture a mutable theme brush with `FindResource(...)` and leave that old brush on the element.
- Subscribe to `ThemeService.ThemeChanged` only when a surface must rebuild generated content or recalculate non-resource state. Unsubscribe when the surface closes.
- Generated `Run`, `TextBlock`, `Border`, icon, calendar, chart, list, and selected-state visuals are included in this rule.
- Test the active selection, hover, pressed, disabled, warning, and error states after a live theme change.
- Light and Dark must be genuine variants, not a dark surface with different accent text.
- Preserve contrast guards, High Contrast, and Reduced Motion behavior.

No first-party calendar day, AI provider, transcript prefix, file selection, button, title, or border may retain the previous wallpaper's color.

### 4.3 Transparency, blur, and materials

- First-party windows use the shared Strata material lifecycle. Do not invent a separate transparency implementation for Terminal, Text, Snip, Settings, Files, pickers, or dialogs.
- `Super + T` toggles material transparency for the active Strata window and whole-window alpha for compatible third-party windows. Do not register an additional app-local global hotkey.
- Blur strength and transparency are independent controls. Nonzero blur must not make a surface unexpectedly opaque.
- A transparent foreground surface must not leave another readable transparent surface directly underneath it. Use the managed occlusion policy so the covered background disappears and returns when the foreground surface closes or moves away.
- Active modal dialogs and Strata open/save/select-folder surfaces take precedence over their owner while active.
- The active floating window hides only windows and widgets it physically covers. It must not blank unrelated areas or cause delayed random disappearance.
- Native white borders on first-party windows are a defect. Use Strata's themed material, window chrome, and active-border rules.
- True fullscreen is intentionally opaque. Glass-expanded mode intentionally retains theme effects and reveal spacing.

### 4.4 Layout and alignment

- Align by visible rendered edges, not raw HWND rectangles. Use DWM frame compensation through the existing geometry policies.
- One and two-app Center Stage must share the same visible width.
- The center lane must align with the rendered top and bottom widget-card envelope at every supported UI scale.
- Text, icons, toggles, and descriptions in repeated settings rows use consistent columns, baselines, padding, and hit targets.
- Segment distinct option groups clearly, but do not wrap every enabled toggle in a glowing highlight box.
- Binary settings use toggles, not checkboxes. Continuous values use sliders.
- Sliders preview their value during pointer drag and commit layout-affecting work when the drag ends, preventing the control from moving under the pointer.
- Buttons with familiar actions may be icon-first, but the icon and tooltip/label must clearly communicate the action.
- Confirmation, wallpaper, picker, and utility windows center in the active monitor's work area and stay above their owner.
- Respect DPI, negative monitor coordinates, narrow secondary displays, and panoramic aspect ratios. Do not hard-code one 2560×1440 composition.

### 4.5 Motion

- Animation is polished feedback, not delay. Keep it responsive and allow Reduced Motion to preserve meaning without large movement.
- Workspace slides travel the complete distance before old state is hidden.
- Slot swaps use directional motion and land on exact target geometry.
- `Super + D` view changes animate windows into the new arrangement while widgets duck or fade; windows must not disappear for seconds and then reappear.
- Widget collapse should occur promptly when pointer/focus leaves, except the persistent AI and YouTube cards described below.
- Do not use timers to paper over ownership/focus bugs when a lifecycle or geometry event exists.

## 5. Product behavior invariants

### 5.1 Center Stage and workspaces

- A workspace contains at most two managed applications.
- One app uses the full center envelope. Two apps use top/bottom slots in Center Stage.
- A third app goes to the next workspace with capacity, Strata follows it, and it receives focus.
- Moving into an occupied destination swaps/exchanges; it never silently discards or stacks an app.
- Closing one window closes exactly one HWND, reflows the survivor, and makes the survivor the next active window.
- Workspace count is 1–10. Removal is blocked if the highest workspace contains an app on any monitor.
- `Super + D` changes only the active monitor's view.
- Secondary monitors default to side-by-side without widgets. Each monitor owns its own workspaces and view state.
- Wallpaper is applied individually per monitor, not stretched across the combined desktop.
- Directional focus skips empty workspaces and never wraps past the last existing window. Directional movement fills intermediate empty locations and follows the active app.

Use `CenterStageLayout`, `StageVerticalEnvelopePolicy`, `WindowFrameGeometry`, `DirectionalNavigationPolicy`, `NewWindowWorkspacePolicy`, `WorkspaceSlideTransition`, and `CenterStageSwapTransition`; do not create a competing layout system.

### 5.2 Floating and expanded modes

- `Super + W` toggles all apps on the active workspace between tiled and floating.
- Floating geometry persists across workspace leave/return.
- Opening or moving another app into that workspace returns it to tiled mode.
- `Ctrl` + left-drag anywhere inside an active floating window moves it, including borderless Strata and third-party windows. Tiled windows must not move.
- Dragging must track the pointer smoothly without fighting the tiler or snapping back during the drag.
- `Super + \` is glass-expanded with small reveal spacing and themed transparency.
- `Super + Shift + \` is true opaque fullscreen.
- Both expanded modes hide the sibling app until Center Stage is restored.

### 5.3 Foreground and occlusion precedence

The active topmost object owns the pixels it covers:

1. Secure Windows surfaces remain Windows-owned.
2. Strata modal dialogs, capture overlays, and active file pickers take precedence over their owner.
3. Active floating windows take precedence over tiled windows and widgets beneath their rectangle.
4. Managed app windows take precedence over widget cards in their rectangle.
5. The top rail ducks when any managed window crosses it and returns when clear; edge reveal remains available where specified.

Hidden surfaces must restore immediately when the occluder dies, closes, hides, changes workspace, or moves clear. Never globally hide all app windows merely because the pointer entered a widget.

### 5.4 Widgets

- AI Command remains at bottom-left and YouTube at bottom-right.
- AI Command and YouTube are persistently expanded. They contract only when another widget needs their space, then restore without pointer tricks.
- Other widgets expand on hover and collapse promptly when interaction ends.
- Hovering AI/YouTube or a video result must not make the center app disappear.
- YouTube search results activate on the first click, stay consistently aligned, and preserve playback when the card yields or collapses.
- The video frame is centered and correctly fitted. Portrait/Shorts previews must not show baked-in dark side bars when a crop-capable source exists.
- AI provider selection is single-click, visibly unambiguous, and live-theme reactive. Transcript role labels and status content update with the palette.
- Weather expansion must show all seven full weekday names without clipping.
- The calendar highlights the actual current day, updates across midnight, and uses the current theme accent.
- Performance/process close actions must report refusal or persistence honestly; do not claim a process ended because a close request was sent.
- Audio spectrum controls should operate YouTube first when applicable and otherwise fall back to system media transport.

### 5.5 Top rail and application tray

- The application tray is one down-chevron, first in the right-side status group before network.
- Do not place one permanent top-rail icon per background application.
- Group tray entries by base product/company, retain the real icon in the dropdown, and expose publisher/native subactions through that base row.
- Filter Windows hardware carrier icons from the application tray; Bluetooth devices belong in Bluetooth UI.
- Network shows Wi-Fi, wired LAN, or offline state accurately. Battery appears when applicable.
- The Snip icon enters rectangle capture on the first click.

### 5.6 First-party apps, dialogs, and files

- Strata Terminal, Text, Snip, Settings, Files, wallpaper chooser, keybind viewer, and dialogs share theme, material, shadow, border, focus, workspace, and `Super + T` behavior.
- First-party open/save/select-folder workflows use `StrataFilePickerService` and the complete Strata Files surface. Do not add `OpenFileDialog`, `SaveFileDialog`, or Explorer as a shortcut.
- First-party confirmations/errors use `StrataDialogWindow` or the current themed prompt system, not `MessageBox`.
- Strata-owned singleton apps should rehome and focus their existing window on the active workspace instead of creating duplicates.
- `Super + Q` closes first-party utility windows consistently, including wallpaper selection.
- Strata apps take precedence over similarly named Windows apps in launcher search.
- Files uses `FileItem` templates; never bind an untemplated object and display `StrataShell.Models.FileItem`.
- Details and icon views, theme-colored selection, path entry, search, type-to-select, thumbnails, hidden files, properties, Recycle Bin, and redirected/network Known Folders must remain functional.
- Large folders enumerate and thumbnail asynchronously with cancellation, visible-first priority, bounded workers, and no UI-thread decode loops.

### 5.7 Hardware and protected Windows boundaries

- Prefer in-Strata Wi-Fi, Bluetooth, audio, display, startup, tray, and power controls.
- Keep rail panels and Settings backed by the same services and state.
- Never collect or persist Wi-Fi passwords beyond the temporary Windows enrollment flow.
- UAC, sign-in, `Ctrl + Alt + Delete`, secure pairing/PIN consent, drivers, and Windows Recovery stay Windows-owned.
- If an `ms-settings:` page fails without Explorer, use a supported direct package/control-panel route or a tightly scoped temporary Explorer bridge. Do not leave Explorer running or freeze the shell while waiting.

## 6. Keybinding contract

`ShellCoordinator.BuildBindings()` is the runtime authority. `docs/KEYBINDS.md` and the searchable `Super + K` view must match it.

The code enforces 88 unique bindings and rejects retired duplicates. Do not add, remove, or remap a chord unless the user explicitly requests it. Never add an app-local system-wide hotkey for an action the shell already owns.

Canonical bindings that must remain unique:

| Action | Binding |
|---|---|
| Browser | `Super + B` |
| Close active window | `Super + Q` |
| Strata Files | `Super + F` |
| Settings | `Super + S` |
| Strata Terminal | `Super + Enter` |
| Strata Snip | `Super + Ctrl + S` |
| Transparency | `Super + T` |
| Tiled/floating workspace | `Super + W` |
| Per-monitor desktop view | `Super + D` |
| Glass-expanded | `Super + \` |
| True fullscreen | `Super + Shift + \` |
| Hardware/system panel | `Super + Ctrl + H` |
| Emergency Explorer recovery | `Ctrl + Alt + Shift + Delete` |

When bindings change by explicit request:

1. Update `BuildBindings()` and its validation policy.
2. Update `docs/KEYBINDS.md`, README essentials, Settings/keybind copy, and any launcher labels.
3. Search the repository for the old chord.
4. Check duplicates and retired chords.
5. Verify the low-level key router, modifier release, screensaver dismissal, and active privilege level.

## 7. Architecture and ownership

`ShellCoordinator` is the composition root. Extend an existing owner instead of creating a second global authority.

| Concern | Primary owner |
|---|---|
| Process modes and startup | `App.xaml.cs` |
| Composition, global routes, bindings | `Services/ShellCoordinator.cs` |
| Settings and migrations | `Models/ShellSettings.cs`, `Services/SettingsService.cs` |
| Wallpaper palette and live theme | `Services/ThemeService.cs`, `PaletteExtractor.cs`, `Themes/Controls.xaml` |
| External HWND discovery/layout/focus | `Services/WindowManagerService.cs` and policy classes |
| Win32/DWM/core audio boundaries | `Interop/` |
| Hardware/session routes | `Services/SystemControlService.cs` |
| App discovery and packaged activation | `AppDiscoveryService`, `PackagedAppService` |
| File management and picker modes | `Views/FileManagerWindow*`, `Apps/Common/StrataFilePicker*` |
| First-party Terminal/Text/Snip | `Apps/Terminal`, `Apps/TextEditor`, `Apps/Screenshot` |
| Widgets | `Views/DesktopWindow*`, focused widget services/policies |
| Idle screensaver | `IdleScreensaverService`, `Screensaver/`, `ScreensaverWindow` |
| Recovery and install validation | `ShellRecoveryService`, `scripts/`, `StrataMaintenance`, `StrataSetup` |

Keep policy calculations deterministic and separate from Win32 side effects where practical. Add or extend policy self-tests when changing geometry, routing, bounds, debounce, state transition, or archive/security rules.

## 8. Settings rules

- `ShellSettings.SchemaVersion` is versioned state. Adding or changing a persistent property requires a migration/default decision and normalization where appropriate.
- Never make an older or partially missing JSON file prevent shell startup.
- Use `SettingsService.Save()` when listeners must apply a change; use `Persist()` only when intentionally avoiding a live `SettingsChanged` broadcast.
- Keep `installer/Defaults/settings.safe.json`, `docs/SETTINGS.md`, `docs/SHIPPED_DEFAULTS.md`, installer validation, and reset behavior synchronized with schema changes.
- Fresh-install defaults must not retain weather location, AI working directory/session state, history, machine IDs, credentials, account identifiers, or user-specific paths.
- Organize Settings by user intent. Avoid duplicate entry points and unrelated controls in About.
- Use clear headings, short explanations, aligned icons/text, right-aligned toggles, and restrained containers.

## 9. Performance and reliability expectations

- Prefer events, file watchers, hooks, and coalesced/debounced refreshes over high-frequency polling.
- Never perform large directory enumeration, image decode, network calls, CLI work, or device discovery synchronously on the UI thread.
- Cancel stale search, thumbnail, wallpaper, weather, and playback operations.
- Bound caches, worker counts, retry loops, and archive extraction sizes.
- Treat filenames, app metadata, CLI output, tray registrations, web content, and logs as untrusted input.
- A lifecycle race should be fixed as a state transition or transaction, not by adding increasingly long arbitrary delays.
- One user action should dispatch one destructive or close operation. Guard duplicate native messages and make the next meaningful surface active.
- Respect applications that refuse geometry, opacity, close, or caption changes; log and degrade rather than fighting them every frame.

## 10. Safety and recovery

### Never do these without explicit user authority

- Do not run `--shell` over the user's active Strata session.
- Do not run `--critical-ui-test` or UI automation that opens/takes over shell surfaces while the user is working.
- Do not launch a second desktop shell on top of the current shell.
- Do not activate a development build as the default shell.
- Do not sign out, restart Windows, restart Strata, kill Explorer, or terminate the active shell merely to make a new build visible.
- Do not modify Shell Launcher, current-user shell policy, Winlogon, or recovery state outside the reviewed installer/maintenance routes.

Safe default development mode is `--preview`. Coordinate any active-shell or installed-build acceptance, state what will be interrupted, and preserve open work.

Never weaken or remove:

- the non-`Super` emergency recovery chord;
- `%USERPROFILE%\Strata Recovery\Return-To-Explorer.cmd`;
- the immutable local release requirement;
- the edition-aware activation route;
- the Explorer fallback;
- the pre-activation self-test; or
- the three-failure bootstrap watchdog.

Use native PowerShell path handling for destructive file work. Resolve and validate exact targets before recursive removal. Never clean a workspace, release root, home directory, or broad computed path.

## 11. Privacy, secrets, and assets

Never commit or publish:

- `%LOCALAPPDATA%\StrataShell` state, `settings.json`, recovery JSON, logs, caches, or diagnostics;
- SIDs, account names, machine identifiers, personal paths, recent files, weather location, or hardware fingerprints;
- GitHub tokens, AI credentials, cookies, passwords, or WebView2 profiles;
- generated `bin`, `obj`, `artifacts`, releases, temporary captures, or PDBs;
### 11.1 Release, update, and branch pipeline

Strata uses a privacy-protecting split-repository architecture:

- **Private source repository** (`https://github.com/ManiaxMax/StrataShell`): Holds all C# source code, WPF views, tests, and development history. This repository is private.
- **Public distribution repository** (`https://github.com/ManiaxMax/StrataShell-Releases`): Hosts compiled binary installers (`.exe`), portable archives (`.zip`), checksums (`.sha256`), and release manifests for login-free user updates.
- **ABSOLUTE RULE: NEVER upload source code to the update repo.** If the user asks to "upload to update channel" or "push an update," it means: build and package the binary release from project source, commit/push source to the private main repository, and publish only compiled release assets to the update repository.
- **WALLPAPER INVARIANT: NEVER commit or package wallpapers without the `Strata_` prefix.** Only the 40 approved 4K Strata wallpapers (20 Light in `wallpapers/WallpapersLight/`, 20 Dark in `wallpapers/WallpapersDark/`) and `strata-signal.png` may exist in the repository or installer packages.

#### Permanent branches and channel mapping

Both repositories maintain two permanent branches:

| Repository | `preview` branch | `main` branch |
|---|---|---|
| `ManiaxMax/StrataShell` (Private Source) | Active feature/preview development source | Reviewed stable source code |
| `ManiaxMax/StrataShell-Releases` (Public Releases) | Target commitish for Preview channel releases (`prerelease: true`) | Target commitish for Stable channel releases (`make_latest: true`) |

#### Update interfaces and channel policy

- **Strata Settings (`Settings > Updates`)**: Supports both **STABLE** and **PREVIEW** channels. Users can switch channels freely and click "Update Now" to install without leaving Strata.
- **Strata Launcher (`Strata Launcher > Update Strata`)**: Strictly checks the **STABLE** channel (`/releases/latest`). Preview releases are never installed from the external launcher unless manually selected.

#### Mandatory end-to-end workflow for agents

When the user asks to make a change and push it to the update channel:

1. **Implement and verify locally:**
   - Make the required source code edits in `C:\Projects\StrataShell`.
   - Build all projects: `dotnet build .\src\StrataShell\StrataShell.csproj -c Release`
   - Run the quiet self-test: `.\src\StrataShell\bin\Release\net9.0-windows10.0.26100.0\win-x64\StrataShell.exe --self-test --quiet`
2. **Push source to private repository (`preview` branch):**
   - Commit changes: `git commit -m "..."`
   - Push to preview: `git push origin preview`
3. **Build release packages directly from project source:**
   - Run: `powershell -NoProfile -ExecutionPolicy Bypass -File .\scripts\Build-StrataInstaller.ps1 -Version <Version>`
   - This automatically compiles `StrataShell`, `StrataLauncher`, and `StrataSetup` from source, packages the 40 Strata wallpapers, generates `StrataShell-Setup-<Version>-win-x64.exe`, creates the portable bundle `StrataShell-<Version>.zip`, writes `SHA256SUMS.txt`, and generates `VALIDATION_REPORT.json`.
4. **Publish to Preview Channel (for testing):**
   - Run: `powershell -NoProfile -ExecutionPolicy Bypass -File .\scripts\Publish-StrataGitHubRelease.ps1 -Version <Version> -TargetCommit preview -Prerelease`
   - The user tests the build live in Strata via: `Settings > Updates > Preview Channel > Check for updates > Update Now`.
5. **Promote to Stable Channel (when approved):**
   - Merge or push source to `main` on the private repository: `git checkout main; git merge preview; git push origin main`.
   - Publish to stable: `powershell -NoProfile -ExecutionPolicy Bypass -File .\scripts\Publish-StrataGitHubRelease.ps1 -Version <Version> -TargetCommit main`.
   - The user on the **STABLE** channel can now check for updates and receive the verified release.

## 12. Git and concurrent-agent rules

- Inspect `git status`, current branch, upstream, and recent history before editing.
- Preserve all unrelated tracked and untracked work. Never use `git reset --hard`, broad checkout/revert, or mass clean to simplify the tree.
- Develop on a named feature/fix/docs branch. Do not directly replace `main`, force-push, rewrite shared history, or merge until the feature has an explicit acceptance path.
- Commit only files relevant to the requested change.
- Do not stage, commit, branch, merge, tag, publish, or push unless the user asked or the established workflow explicitly requires it.
- If another agent owns central integration, keep work isolated and provide an executable Markdown handoff with file-level and hunk-level instructions.
- Do not wholesale-copy `App.xaml.cs`, `ShellCoordinator.cs`, `ThemeService.cs`, `WindowEffects.cs`, or other central files from an older branch. Reconcile current code manually.
- Re-read `ShellCoordinator.BuildBindings()` immediately before merging app/hotkey work.

## 13. Implementation workflow

1. **Inspect.** Confirm repository identity, branch, dirty state, relevant docs, source owner, settings, tests, and recent changes.
2. **Reproduce.** Capture exact steps, active build path/version, logs, window state, monitor/workspace/view, and screenshot where useful.
3. **Define acceptance.** State the visible outcome and regression cases before editing.
4. **Fix the owner.** Change the service/policy that owns the state instead of patching several views independently.
5. **Check siblings.** A stale theme brush, focus race, or modal-occlusion bug often exists in other generated surfaces.
6. **Build narrowly.** Run the quickest relevant compile/check first.
7. **Verify broadly.** Expand to the self-test and safe visual acceptance based on risk.
8. **Package only when requested or required.** Preserve the current user's session.
9. **Document.** Update contracts, backlog status/evidence, release notes, and handoff details.
10. **Report precisely.** Say what changed, what ran, what remains, and which build is actually active.

## 14. Build and verification commands

Restore and build:

```powershell
dotnet restore .\src\StrataShell\StrataShell.csproj
dotnet build .\src\StrataShell\StrataShell.csproj -c Release --no-restore --nologo
```

Run the noninteractive suite after a Release build:

```powershell
.\src\StrataShell\bin\Release\net9.0-windows10.0.26100.0\win-x64\StrataShell.exe --self-test --quiet
```

Read the result from:

```text
%LOCALAPPDATA%\StrataShell\Recovery\self-test.json
```

Do not hard-code an old self-test count in a report. Read `Success`, enumerate failed checks, and report the current count from the generated JSON.

Safe interactive preview:

```powershell
dotnet run --project .\src\StrataShell\StrataShell.csproj -- --preview
```

Do not start preview if another preview or shell instance would conflict. Do not use `--critical-ui-test` without coordinating because it opens and manipulates real first-party surfaces.

### Installer and update release paths

Strata uses a split repository architecture for privacy-safe public distribution:

- **Private source repository** (`ManiaxMax/StrataShell`): Holds all private source code, tests, and commit history.
- **Public distribution repository** (`ManiaxMax/StrataShell-Releases`): Hosts only compiled installers (`.exe`), portable archives (`.zip`), manifests, and release notes for unauthenticated user updates. Never push source code to the distribution repo.

Packaging and distribution commands:

- `scripts/Build-InstallerBundle.ps1`: Builds the current source into the updater/self-contained bundle (`StrataShell-<Version>.zip`).
- `scripts/Build-StrataInstaller.ps1`: Builds the polished Setup/Launcher package (`StrataShell-Setup-<Version>-win-x64.exe`) and manifests.
- `scripts/Test-StrataInstaller.ps1`: Validates the installer in an isolated temporary root without modifying the production system.
- `scripts/Publish-StrataGitHubRelease.ps1`: Uploads the validated binaries, zip bundle, checksums, and release manifest to the public `ManiaxMax/StrataShell-Releases` repository.

Activation and installation are external-state changes. Do not run `Install-StrataShell.ps1 -Activate`, graphical Setup, shell-policy helpers, or recovery helpers unless explicitly authorized for that task.

## 15. Verification matrix

| Change area | Minimum automated evidence | Required user-visible evidence when safe |
|---|---|---|
| Theme/XAML/material | Release build, quiet self-test | Light, Dark, live wallpaper/theme switch, selection/hover/error states, `Super + T` |
| Window layout/focus/close | Policy checks, quiet self-test | One/two apps, open third, close each order, swap, workspace return, exact focus |
| Floating/occlusion | Policy checks, quiet self-test | Smooth `Ctrl`-drag, overlap/restore, workspace leave/return, top rail behavior |
| Widgets | Build, widget/policy checks | Hover/collapse, persistent AI/YouTube, center app remains visible, scaling |
| YouTube/WebView2 | Build, thumbnail/pointer policy | First-click results, centered player, persistent playback, Shorts previews |
| Files/picker | File operation and picker checks | Local + redirected/network folder, Details/Icon, thumbnails, type-to-select, open/save |
| Keybindings/input | Binding uniqueness/policy checks | Actual chord, modifier release, focus after close, no duplicate action |
| Screensaver | Idle/input policy checks | Manual start, Light/Dark, all dismissal inputs, keybinds immediately after exit |
| Hardware | Deterministic parsing/grouping checks | Real device/connect/disconnect route; protected Windows prompt where required |
| Installer/recovery | Bundle/setup acceptance, hashes, isolated test root | Explicitly coordinated install, next sign-in, recovery and uninstall |

For a visible change, capture a screenshot or short recording at the relevant scale. Inspect the pixels; do not infer appearance from XAML alone.

## 16. Documentation and backlog discipline

- Add new user reports to `docs/STRATA_BACKLOG.md` with reproduction, expected behavior, evidence, and completion criteria when they are not fixed immediately.
- Mark an item `[x]` only after its completion condition is observed in the correct environment. “Built” is not “installed and verified.”
- Keep `README.md`, `docs/FEATURES.md`, `docs/SETTINGS.md`, `docs/KEYBINDS.md`, architecture, installer docs, release notes, and in-app labels consistent with changed behavior.
- Preserve explicit limitations. Do not advertise untested multi-monitor, accessibility, codec, hardware, secure-surface, or third-party compatibility as complete.
- When a source-to-installed gap exists, document the exact release ID and activation state.

## 17. Handoff format

Every substantial handoff should include:

1. **Outcome:** the user-visible result first.
2. **Root cause:** concise, evidence-backed explanation.
3. **Files changed:** scoped list and why each changed.
4. **Verification:** exact commands, exit codes, check counts, screenshots/manual scenarios, and machine-dependent gaps.
5. **Build state:** source commit/branch, packaged release ID, configured shell target, and actually running executable path.
6. **Safety/recovery impact:** whether shell, Explorer, settings, credentials, or Windows policy changed.
7. **Remaining work:** only real caveats or unverified acceptance—not generic filler.

Never claim success from status text, a commit message, generated metadata, or a previous agent's report without checking the current artifact and environment.

## 18. Final agent checklist

Before stopping, verify:

- Did I implement the user's latest request rather than an older assumption?
- Did I preserve Strata's wallpaper-driven, futuristic, non-Windows visual identity?
- Are layout, icons, text, controls, and hit targets aligned and readable?
- Do all generated and static first-party elements update live with the theme?
- Does transparency preserve legibility and restore covered surfaces correctly?
- Did I preserve Center Stage, two-app capacity, per-monitor state, and focus continuity?
- Did I leave the canonical key map and recovery routes intact?
- Did I avoid launching another shell or interrupting the user's active session?
- Did I protect settings, credentials, personal paths, wallpaper ownership, and recovery state?
- Did I run and truthfully report the strongest safe verification available?
- Did I update documentation and backlog evidence where the contract changed?
- Did I clearly distinguish source, installed, configured, and running versions?

If any answer is no, the work is not ready to call complete.
