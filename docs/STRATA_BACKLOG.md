# STRATA — cleanup and native wireless follow-up

## September 13 — requested fixes and improvements

### Sphered — per-monitor Sphere spaces

- [~] September 17 lock/display recovery: rapid Sphered enter/exit after unlock could leave a closed generation's per-monitor restoration incomplete, producing a black secondary desktop with no rail and a stale translucent Sphere surface. The coordinator now generation-checks asynchronous exits, closes every tracked or still-open Sphere surface, prevents an old close callback from removing its replacement, reparents monitor widgets, and explicitly reconciles each desktop and rail after the final exit. A scoped Release build passed with zero warnings/errors, quiet self-test passed 151/151, and private-desktop Sphere acceptance passed 169/169 including forced restoration of a hidden secondary rail (`artifacts/sphere-display-restoration/report.json`). The repository's unrelated in-progress Audio Editor currently blocks the unfiltered project build. Installed lock/two-monitor acceptance remains pending.

- [~] Games detected through Steam manifests or Unity/Unreal markers now start opaque, with per-tab user toggles preserved. Two-display Files round trips through Floating and Tiled passed, including docking cancellation overlapping a mode exit; 125 private-desktop checks and a zero-warning Release build passed. The monitor fix records the destination display for windows created elsewhere before docking. Installed acceptance remains pending. [Evidence](SPHERED.md).

- [~] Second-monitor game launch: share launcher/request ownership between native show observation and periodic window collection; discover Steam URL game installations across libraries and retain their requested space through delayed startup. Removed the right-click monitor submenu at the owner's request. The navigation row now appears only for websites and web searches. Release build, 150 self-tests and 113 isolated Sphere checks passed; live Steam-library launches on the updated second Sphere remain pending. Automatic internal borderless conversion, including Quake Champions, is not yet implemented. [Evidence](SPHERED.md).

- [~] Urgent display-lock/tab-return follow-up: shared title handling and native geometry now keep Sphere monitor-sized; tab layout/raising/focus restoration is coordinated; Sphere-created STRATA apps regain desktop registration and taskbar/chrome state. The header is compacted and saved Sphered startup suppresses widgets, edges and rails before showing. Release build, 150 self-tests and 85 private-desktop checks passed, including external Chromium live frames/screenshots and repeated display-lock attempts. Installed target-app/physical-input and sign-in acceptance remain pending. [Evidence](SPHERED.md).

- [~] Source: Sphered mode, embedded live status controls, monitor-specific docking/undocking, several independent two-tab groups, searchable grouping, monitor transfer, bounded native hosting and force close. Steam → Balatro, NVIDIA Control Panel and Windows Control Panel passed private-desktop hosting checks. [Contract and evidence](SPHERED.md).
- [~] Follow-up source fixes: preserve renderer lifecycle across tab switches; complete native restoration before undocking; remove monitor-dependent Sphere size caps and false fullscreen classification; restore menus/mode notifications; per-tab transparency; search-only home and shell-utility filtering; mode-specific shortcut routing/list/editor; prevent pane fullscreen. Release build passed with zero warnings/errors; 150 self-tests and 58 private-desktop Sphere checks passed. Regression acceptance is recorded under `artifacts/sphere-regression-verified`. Installed Steam/ChatGPT/NVIDIA/Balatro rendering and physical input remain unverified on the new build; the running Preview was not interrupted.
- [ ] Installed acceptance remains: physical keyboard/pointer, live monitor transitions and protected/exclusive-fullscreen applications. Source and private-desktop evidence do not modify or certify the running shell.

All items below are implemented in the maintained source checkout. `[~]` means source-built and locally checked; installed acceptance remains open. Source branch: `preview`, based on `3314703`. Running release `1.0.18` was left unchanged. No commit, publication, installation, shell restart, CLI installation or user PATH change was performed.

- [~] Fullscreen and directional-focus shortcuts in Floating; existing close, workspace and monitor routes remain available. Tiled retains its three-stage expansion cycle.
- [~] Command/application launcher allowed during fullscreen; top/bottom edge reveal exposes the rail/dock, and clicking a rail selects its monitor.
- [~] Capture suspends layout and overlap dimming for all managed apps, including cancellation/failure cleanup. Snip auto-copy is enabled by default and its toggle is in Snip's own Settings popup.
- [~] Settings search, including compact layouts; both Floating and Tiled options remain available with mode labels.
- [~] Widgets on all monitors moved to Widgets and available in either mode. Enabling reveals secondary slots; Ctrl + left-click drag moves/swaps widgets across monitors. Saved assignments fall back to primary while disabled/disconnected and return when available.
- [~] Four selectable Task Manager performance modules, including GPU usage and supported temperatures, plus discovered core/drive/adapter metrics. Existing CPU/RAM/Network/Processes selections migrate; default metrics retain the lightweight sampler.
- [~] Terminal + menu creates independent CMD/PowerShell tabs. Switching preserves process, output, folder, history and unsent input. Closing/restarting affects its tab; elevation preserves other tabs.
- [~] AI settings expose per-provider dropdowns for model, speed and effort, plus an explicit default-off permission-bypass toggle. Codex uses its local model catalog; Antigravity models can be refreshed; Claude offers model aliases. Unsupported controls are disabled. Install/repair PATH actions detect each CLI and use its official Windows installer. CLI default makes no resolved-model claim.
- [~] Follow-up drag fix activates mouse capture for the gesture, tracks physical coordinates across monitors, and retains destination slots through layout refreshes. Physical acceptance remains separate from isolated coordinate tests.
- [~] Performance widget Reset defaults restores its original four modules and update interval. YouTube Home cancels pending search/playback and returns to the initial widget view.
- [~] Both launcher modes offer right-click Pin to dock / Unpin from dock for applications. The keybinding editor can replace built-in application targets and record a chord while suppressing its normal action; occupied combinations remain rejected.
- [~] STRATA Calendar adds local appointments, all-day events, editing/deletion, date popups and upcoming events in the widget/rail clock. Secondary palette color marks events. Sync and account sign-in were explicitly canceled and are excluded.
- [~] Fresh-install floating background transparency is 0%; updates retain customized opacity. Schema 45 defaults contain no display identities, models, credentials or bypass opt-ins.

Validation: Release compilation with zero warnings/errors; 149/149 quiet self-checks; 583/583 existing private-desktop regression checks; 29/29 focused checks including real CMD/PowerShell process isolation, fullscreen geometry restoration, capture scope, GPU rendering and cross-monitor coordinate/drop/restore behavior; 30/30 setup/portable default and existing-profile preservation checks. Light/Dark Settings and Terminal images plus compact search and GPU module captures were inspected. Evidence is under `artifacts/requested-improvements-monitor`, `artifacts/requested-improvements-verified`, `artifacts/desktop-mode-requested-improvements` and `artifacts/requested-improvements-defaults`.

September 13 follow-up validation: the expanded focused fixture passes 62/62 checks under `artifacts/requested-followups-release-check`, including provider dropdown persistence and argument routing, Snip settings, performance reset, YouTube Home, launcher pins, replacement-app shortcuts, recorder consumption/release/focus-loss, local Calendar CRUD and view updates, and secondary drop slots surviving refresh. Calendar Light/Dark, appointment editor/date popup, Snip settings and AI dropdown images were inspected. The Release build has zero warnings/errors; 149/149 quiet self-checks and 583/583 desktop-mode checks also pass (`artifacts/desktop-mode-followups`). The private desktop is intentionally inactive, so native foreground capture and physical keystrokes remain installed acceptance items; recorder tests inject foreground ownership and exercise the production recording state machine.

An initial concurrent self-test run timed out in the existing ConPTY check; the serial rerun passed. The first focused search assertion omitted string-valued button content; the corrected assertion and compact visual inspection pass.

Remaining acceptance: install the candidate only when authorized, then exercise physical global shortcuts/rail reveal over actual games (exclusive fullscreen can yield or minimize on focus), real Snip clipboard/capture behavior, ongoing widget content during cross-monitor moves, and provider sign-in/model execution/installers. The isolated checks do not establish those installed behaviors. Completion requires observing them in the candidate build.

September 12 keybinding editor follow-up: every STRATA catalog binding is editable, including editor/recovery access, locking, app switching, volume controls and actions for both desktop modes. Occupied combinations are rejected on save/reset, including application shortcuts and physical aliases. Release build passes with zero warnings/errors; 148 self-checks and 15 isolated checks pass. The isolated editor saves, preserves unchanged chords, rejects duplicates without changing persisted settings, and resets all 75 catalog actions; Light/Dark captures and report are under `artifacts/keybinding-editor-acceptance`. Running release `20260910-111554` is unchanged; packaging, installation and physical hotkey acceptance remain pending.

September 6 YouTube startup follow-up: custom saved slots exposed column detection through an unconnected visual tree. Column ownership now uses the card's assigned slot before templates load. The owner's Clock-left/YouTube-right arrangement passes no-hover startup at height 385, plus all 558 isolated checks (`artifacts/youtube-slot-startup-acceptance/report.json`); Release build passes. Running release `20260906-091745` is unchanged; this source fix still needs deployment and physical startup acceptance.

Updated September 5, 2026. Phase 5 is approved and active; see [the Phase 5 record](PHASE_5_ACCEPTANCE.md).

This replaces the previous backlog. The old agenda is retained only as [historical reference](archive/STRATA_BACKLOG_BEFORE_CLEANUP_2026-09-05.md); its open items are not commitments for this pass. Laptop-specific investigation is removed from the active scope. Publisher signing is deferred indefinitely and is not a prerequisite for this cleanup.

| Item | Scope | Status |
|---|---|---|
| 1 | Preserve and verify the reported Text/Snip Discard, floating maximize/bar, picker highlight, and tiling workspace fixes | Fixed locally; isolated regressions pass; installed acceptance pending |
| 2 | Review every Settings page, remove stale controls, improve wording and layout, synchronize saved defaults | Implemented and reviewed locally |
| 3 | Dock STRATA icon opens Applications; remove Super + Esc; power always visible; remove CPU bar module | Implemented; local checks pass |
| 4 | Task Manager transparency and covered-window border; remove Users | Implemented; local checks pass; installed acceptance pending |
| 5 | Widget appearance, expansion/collapse, buttons and editors across all slots | September 6 follow-up: reconcile saved locks on first rendered layout and restore retained YouTube state synchronously during startup. Release build and 557 isolated checks pass, including no-hover startup and interrupted restore; report: `artifacts/youtube-startup-acceptance/report.json`. Running installed release `20260906-085537` unchanged; physical startup acceptance pending. |
| 6 | Review animation paths, interruption and frame pacing | Implemented; transition tests pass; physical smoothness remains to assess |
| 7 | Native tray click, double-click, menu and focus behavior, including apps such as Tailscale | Open restores tracked windows or exposes native controls with guarded fallback; 41 tray checks pass; current Open regression needs live acceptance |
| 8 | Replace the old completed/remaining inventory with this pass's evidence and release status | Complete; see the current inventory |
| 9 | Bluetooth discovery arrives late or finds nothing; pair mice, keyboards, headsets and speakers | Streaming AEP discovery and custom pairing implemented; real discovery verified; physical pairing acceptance pending |
| 21 | Bluetooth Disconnect reports success while the device continues working | 2026-09-06: source now tries every radio instead of only the first, no longer treats error 1167 on one radio as proof of disconnection, and requires two disconnected readings across all profiles. Connected or unknown profiles cannot report success. Release build: 0 warnings/errors; 144/144 self-tests pass, including mixed/unknown connection-state checks. Acceptance pending: disconnect the affected physical device, verify it stops working and remains paired, then reconnect; verify automatic reconnect produces an honest result. Not packaged or installed; running release remains 20260906-085537. |
| 10 | Native Wi-Fi disconnect, forget and enable/disable controls | Shared native controls implemented; isolated interaction checks pass; physical connection/radio acceptance pending |
| 11 | Network click menu and Bluetooth tray button; remove routine Explorer dependency | Implemented; menu routing and real discovery checked without Explorer |
| 12 | Native Display, Input, launcher startup selections, Power, Sound and Date + Time settings | Implemented locally with capability limits; see native settings acceptance |
| 13 | Quick Controls/commands, clock/calendar and cleaner status bar | Implemented locally; panel and routing checks pass |
| 14 | Temporary Explorer activity must preserve default shell | Automatic recovery now preserves the selected shell; companion process-tree termination removed |
| 15 | Multiple document windows, grouped bar icons, Close, previews and minimize targets/animation | Implemented locally; installed DWM/animation acceptance remains |
| 16 | High Quality wallpaper switch delay with synchronized frost/theme | Preparation optimized; image/palette/frost still publish together with matching crossfade timing |
| 17 | Browser toolbar consolidation | One Downloads control; Bookmarks/History on main toolbar; Menu alone beside tabs; developer-tools toolbar button removed |
| 18 | Speaker icon opens sound controls instead of muting | Corrected routing; explicit volume readings and Mute/Unmute controls; source/isolated checks pass |
| 19 | Recovery dialog while opening desktop apps: tray list changes during refresh | Root cause confirmed in installed log; stable registration snapshots implemented; 52 focused checks and 144 self-tests pass; deployment and live reproduction still pending |
| 20 | STRATA Files shell crash after repeated List/Icons switches | Installed log confirms NullReferenceException in VirtualizingWrapPanel.RealizeRange. Initialize the panel generator through InternalChildren before use and skip detached items hosts; avoid rebuilding an already applied view on settings notifications. Isolated WPF reproduction failed before the fix and passed 50 populated Icons/Details cycles plus detached-panel measurements afterward (2026-09-06, .NET 9 harness). Full build/self-test blocked by missing required .NET 10.0.400 SDK. Completion requires a supported build and repeated switching in installed Files and picker surfaces without recovery/crash, preserving content and selection. Not packaged or installed. |

The native settings and latest bar/browser/wallpaper pass is recorded in [Native settings acceptance](NATIVE_SETTINGS_ACCEPTANCE.md), including supported capabilities, tests and remaining physical checks.

Local source is `C:\Projects\StrataShell`. A source fix, a passing isolated test, an installed build, and an observed live result are separate milestones. The user has now authorized hands-on PC testing; documentation publication is now authorized; binary release publication is separate. Current evidence and testing limits are in [Follow-up acceptance](FOLLOWUP_ACCEPTANCE.md).

Tailscale follow-up, September 5: the running `20260905-095305` release returned zero from `SHAppBarMessage(ABM_GETTASKBARPOS)`. Tailscale 1.102.3's Walk dependency checks this before registering its tray icon, so starting the GUI process alone left the app inaccessible. The source host now answers that query before announcing `TaskbarCreated`, reports primary top/bottom bar geometry, and supplies valid initial icon bounds. Open sends a single selection for version-4 publishers, avoiding Tailscale's menu toggle cancelling itself. The isolated tray report (`artifacts/tailscale-tray-20260905/report.json`, local evidence) contains 31 passing checks, including real taskbar queries without Explorer and the menu-toggle regression. Completion still requires installing the candidate and observing Tailscale's native menu, submenus, and focus on the active desktop; connection-state changes require user authorization.

That earlier Tailscale candidate was subsequently superseded: this follow-up observed release `20260905-102604` running, and the user confirmed Tailscale's menu was fixed but reported that Open still did nothing. Current tray behavior and candidate verification supersede the older 31-check activation expectation; see [Follow-up acceptance](FOLLOWUP_ACCEPTANCE.md).

The [current completed/remaining inventory](STRATA_IMPLEMENTED_AND_REMAINING.md) contains the changes, test artifacts and short installed-acceptance list. Phase 5 covers code review, fresh-install defaults and documentation.

Native wireless completion evidence is in [Native wireless acceptance](NATIVE_WIRELESS_ACCEPTANCE.md). Acceptance requires the new candidate to be installed, then pairing a real peripheral and exercising Wi-Fi connection/radio changes. No physical pairing or disconnection was performed during development.

## Current approved follow-up

### Preview update profile routing — September 5

- [x] Reproduced the `20260905-181532` update failure: the portable installer passed `Recovery/settings.json` to the fresh-profile seeder, while STRATA and Setup use the parent data directory. An existing installation without the Stable wallpapers at the portable library path was incorrectly rejected as fresh. The installer now separates its data and recovery paths and preserves the real profile.
- [x] The actual installer assignments/seeding call pass six regressions, including an existing profile with no wallpapers, obsolete Recovery settings, and genuine fresh-install validation. Copied package scripts also pass all six under Windows PowerShell 5.1. Existing defaults/preservation checks pass 22/22 and installer replay policy checks pass. Evidence is local under `artifacts/preview-profile-path/`. Preview publication now gates its packaged scripts on this regression.
- [ ] Publish a newly signed Preview containing the corrected script and confirm the Zenbook update completes. The already-published `20260905-181532` package retains the incorrect path; this source correction does not change that download or the active installation.

### Browser workspace freeze — September 5 owner report

- [ ] Installed acceptance: repeated Browser workspace moves hide the surrounding controls while the webpage remains visible, slow the shell, and can trigger recovery. The owner's new report reopens this laptop investigation despite its earlier deferral. The installed log records repeated roughly two-second delays and a 43.9-second UI responsiveness delay. Closing the affected browser restores responsiveness; the owner also reports eventual recovery without closing it.
- [x] Source correction and isolated regression for the disappearing controls: native `HwndHost` content bypasses WPF-only fades, including lazy Browser/Terminal initialization. The original fade behavior fails the comparison checks; the candidate preserves browser controls during one/two-app and interrupted workspace moves.
- [x] Live local preview acceptance after the focus/visibility follow-up: the owner tested repeated workspace and monitor moves and confirmed **“Smooth, and controls stay visible.”** The exact tested source executable was observed running with `--preview`. Fourteen directional moves and thirteen completed slides recorded 182–330 ms elapsed with no named slow operations or UI exceptions during that preview session. The owner-approved temporary Explorer session preserved shell configuration; no update was installed. See the investigation record for remaining packaged/installed acceptance.
- [ ] Verify the fix in the packaged/installed release. The owner reproduced the original stall on running Preview `20260905-183248`: native bring-to-top/focus calls took about two seconds each, an intermediate inactive-window hide took four seconds, and the dispatcher was delayed 28 seconds. The local follow-up preserves native child focus, avoids input-queue attachment and redundant activation, and keeps a followed window visible throughout workspace/monitor reassignment. A delayed native focus-loss fixture and real visibility-message checks fail on the prior code and pass after correction. Live local preview acceptance passed; packaged/installed acceptance remains open and driver involvement is unproven.

Evidence, candidate state, exact verification and remaining acceptance are in [Browser workspace investigation](BROWSER_WORKSPACE_INVESTIGATION.md).

### Browser close stall — September 5 follow-up

- [x] Reproduced in installed Preview `20260905-194224`: individual closes intermittently take about two seconds, with longer waits as close requests queue. A read-only Windows wait-chain capture sees synchronous cross-process `SendMessage` waits on the shell UI thread. The exact managed/native call and receiver identity remain unconfirmed.
- [x] Local source candidate: dispose Browser controllers before the containing window is destroyed and before detaching individual tab visuals; preserve cancellation and pending-initialization guards. Apply the shared native-content opacity policy to close animations. Private regression checks distinguish the original lifecycle/chrome failures from the corrected source.
- [ ] Signed Preview acceptance of repeated Browser closes. The owner selected private `preview` source transfer for the main computer to build, sign and publish, deferring another live local preview. The close candidate has passed source checks but has not yet been tested live on this laptop. See the [close investigation](BROWSER_WORKSPACE_INVESTIGATION.md#browser-close-follow-up-on-preview-20260905-194224).

[Power and runtime follow-up](POWER_RUNTIME_FOLLOWUP.md) records the .NET 10 migration, Stable-only runtime packaging, power/battery controls, widget/scroll/card fixes, hardware inventory, uninstall cleanup and hang mitigation. Its remaining installed checks supersede earlier blanket statements about missing battery history or an unimplemented runtime migration. Publisher signing and the retired laptop investigations remain deferred.

Bluetooth item 21 follow-up (2026-09-06): Owner tested MX Master 3S and confirmed it still works after Disconnect times out. Read-only PnP inspection confirms a Bluetooth LE device. The previous adapter/confirmation change did not resolve physical disconnection. LE timeout guidance now explains that Windows/other consumers can retain the connection and suggests device power/channel switching; Remove explicitly means unpair. Persistent per-device disabling and re-enabling is not implemented. Hardware acceptance remains open.

## September 7 owner usability reports — local candidate, installed acceptance pending

Implemented in the authoritative checkout on `preview`; no commit, package, installation or shell restart performed. The running installed executables remain in release `20260906-131030`.

| Report | Source change and acceptance still required |
|---|---|
| Tiled windows move before snapping back | Consume native caption/resize hits before movement; reject managed-window move/size commands. Test ordinary third-party, custom-chrome and elevated windows, client controls, and Super + Ctrl movement on the installed build. |
| Recovery repeatedly appears during installers | Runtime log showed Win32 error 5 in ActiveWindowBorderOverlay.CreateSurfaceAt. Treat denied/disappearing popup owners as an unavailable decorative border, expiring the surface instead of raising a UI fault every frame. Test the Audacity upgrade; crash watchdog, heartbeat, fault channel and emergency routes remain intact. |
| Wallpaper Browse drag selection opens Recovery | Runtime log identifies SelectedItems.Clear in a single-selection picker. Single-file mode now bypasses multi-selection drag handling; ordinary Files retains marquee selection. Test both paths in the installed build. |
| Terminal scrollbar ignores theme | Scrollbar colors derive from the terminal palette and update with it. Inspect Light, Dark and a live theme change. |
| Paint colors, eyedropper, rectangular selection | Custom RGB/ARGB/name input and current swatch; eyedropper returns to the last paint tool; select/copy/delete/resize with history. Test pointer selection, clipboard, and dimensions at canvas edges. Resize is through width/height fields, not drag handles. |
| Snip markup colors | Custom pen/highlighter color input; inspect resulting strokes and saved capture. |
| Text font and formatting | Font/size/emphasis/alignment menu for editor appearance. Plain-text storage is preserved; per-range rich-text formatting and saving typography are not implemented. |
| Files footer vanishes when small | Removed zero-height compact footer, scaled footer contents and raised minimum size to 640 × 360 DIPs. Inspect ordinary and picker windows at minimum size and representative DPI. |
| Applications populate slowly | Start discovery after surfaces appear, share in-flight scans, and discover packaged/desktop catalogs concurrently off-thread. Measure cold/warm launcher timing in the installed build. |

Validation: Release build succeeded with zero warnings/errors. The current .NET 10 executable's quiet self-test exited 0 with Success=True, 145/145 checks, including selection enlargement/shrink, unchanged surrounding pixels, undo/redo and tiled hit-test button exclusions. `git diff --check` passed. No installed visual or physical pointer acceptance is claimed.

### September 7 Paint/Snip/Text usability revision

Supersedes the color-entry boxes, numeric selection-resize fields and typography menu described above. Paint and Snip now have eight quick swatches and a visual custom-color popup. Paint uses a black/white selection border and corner handle, dragging for move/resize, and Copy/Cut/Paste/Delete buttons plus keyboard shortcuts. Clipboard images paste into an active selection, fitting within the canvas; moving pasted content restores its prior background. Text uses a labeled Notepad-style font panel with family, style, size, sample and OK/Cancel.

Evidence: Release build succeeded with zero warnings/errors; quiet self-test exited 0 with 145/145 checks. Regression coverage includes selection move/resize, pasted-background preservation, undo/redo and HSV conversion. Offscreen WPF renders inspected in Light/Dark under `artifacts/usability-preview/` caught and corrected hidden palette controls and blank initial font selections. The harness never showed windows or touched the clipboard. Installed physical drag/clipboard/dropdown and live-theme acceptance remain pending. No installation, restart or publication performed.

### September 8: Files USB eject (source candidate)

Added a drive-sidebar context menu with Eject USB device. Backing-disk WMI associations and USB device ancestry determine eligibility; system roots and multi-disk mappings are rejected. Requests revalidate device identity, serialize concurrent ejects, and use Windows Plug and Play safe removal with veto reporting. No force-unmount or process termination. Files blocks its active transfers and leaves the current location to release its own watcher/previews before requesting removal.

Release build passed with zero warnings/errors. Quiet self-test exited 0: Success=True, 146/146 checks, including strict drive-root validation and success/busy/refused result handling. No device was ejected during development. Pending: installed menu interaction, real USB flash/UAS disk and multi-volume removal, busy-device veto and reconnection. Not packaged, installed, restarted, committed or published.

### September 8: Wi-Fi Automatically connect (source candidate)

Added the same Automatically connect checkbox to network cards and Saved Connections in the shared WirelessPanel used by Settings and the rail dropdown. Saved values come from Windows WLAN profiles; new enrollment accepts the checkbox preference. Profile edits verify readback, preserve encrypted credentials and profile scope, and refuse organization-managed profiles. Unreadable preferences remain disabled. No live adapter or network preference was changed during development.

Release build: zero warnings/errors. Quiet self-test: exit 0, Success=True, 147/147 checks, including auto/manual enrollment, encrypted-profile roundtrip preservation and invalid-profile handling. USB eject changes from the preceding task were preserved. Pending: installed UI/pointer testing on both surfaces and physical automatic reconnection. Not installed, restarted, committed or published.

### September 8: selectable wallpaper change animations (source candidate)

Added Glow Sweep, Crossfade, Slide, Wipe, Zoom and None to Wallpaper settings. A shared animator drives desktop and lightweight monitor wallpaper images, cancels stale callbacks on interruption, resets transform/clip/opacity state and bypasses motion for Reduced Motion/Low effects. The existing desktop glow/sweep embellishment runs only for Glow Sweep. Profile schema 41 and installer defaults preserve that mode by default; normalization rejects unknown values.

Release build: zero warnings/errors. Quiet self-test: exit 0, Success=True, 147/147 checks, including each animation setup, interruption by None, reset state, Reduced Motion and Low effects. An offscreen WPF harness exercised all six animations through completion and captured/inspected intermediate frames (`artifacts/wallpaper-animation-preview/transitions.png`). These synthetic frames test the shared wallpaper image effect, not the desktop glow embellishment. No live shell or wallpaper was changed; installed multi-monitor transitions and settings interaction remain pending. Earlier USB and Wi-Fi work was preserved. Not packaged, installed, restarted, committed or published.

### 2026-09-08: STRATA app flicker during tiled workspace switches

- [~] Source fix; installed visual acceptance pending. Report: Files, Terminal and other STRATA windows flicker/glitch when changing tiled workspaces.
- Evidence: workspace animation used the resize visual policy, which collapsed frost and removed effects at entry, restoring them at exit. Incoming windows were also shown before their initial transition opacity was applied.
- Change: workspace slides preserve live frost alignment and effects; incoming opacity is initialized before showing the window. Resize/interactive movement retain their existing policy.
- Validation: Release build, zero warnings/errors; quiet self-test 147/147, including workspace material preservation and opacity restoration. Running build observed as 20260907-220635; source changes have not been installed or visually accepted.
- Acceptance: verify Files and Terminal with one/two tiled apps, empty workspace, rapid forward/back switches, Light/Dark themes and actual monitor scaling; no backdrop flashes and exact final geometry/focus.

### 2026-09-08: Carried-window direction and Files workspace refresh

- [~] Source corrected; installed visual acceptance pending. Moving an app right must bring that app in from the left, and moving left must bring it in from the right. Other destination apps retain their workspace arrival direction.
- The workspace transition now identifies the carried HWND separately, reverses only its incoming direction, and computes clearance for that side. Normal workspace navigation is unchanged.
- Files previously rescanned and replaced its item list on visibility and activation, including workspace return. Watched folders now retain their list unless a watcher event requests refresh; changes while hidden remain pending. Archive/Recycle Bin and unavailable watchers retain refresh-on-return. Loaded no longer repeats initial navigation.
- Release build: zero warnings/errors. Quiet self-test: 147/147, including carried arrival geometry and clean/dirty/unwatched refresh cases.
- Acceptance pending: repeated left/right moves into empty and occupied workspaces, Files selection/scroll retention on clean return, refresh after hidden folder changes, and one/two-app visual smoothness. No publication, installation or shell restart performed for this change.

### 2026-09-08: Instant app moves between workspaces

- [~] Owner reports Files still jumps when moving between workspaces. Clarified scope: disable animation only when carrying an app; preserve animation when browsing workspaces.
- SwitchWorkspaceCore uses the existing immediate switch path for carried-window moves, including floating moves that follow the app. Ordinary workspace navigation keeps its slide/fade, respecting Reduced Motion and fullscreen. Same-workspace slot swaps retain their existing behavior.
- Live acceptance pending: compare Files across empty/occupied workspaces and repeated left/right moves after installing a build containing this change. No install or shell restart performed.

### 2026-09-08: Quicker tiled window swaps

- [~] Shortened same-workspace slot swaps by about 20%: default duration 260 to 210 ms; speed-adjusted bounds 160-420 to 130-340 ms. Existing Animation Speed preference, easing, arc and final placement remain in use.
- Source-only timing adjustment; live feel awaits owner testing. Workspace browsing and cross-workspace app moves retain their existing behavior.

### 2026-09-08: Reversible live Composition backdrop prototype

- [~] Opt-in Experimental Live Backdrop in UI & Theme > Interface; schema 42, default off. Turning it off immediately returns to wallpaper frost. No private profile or installed shell changes.
- Uses a Windows Composition host-backdrop brush on a bottom-layer DesktopWindowTarget behind transparent WPF content. No screenshots, capture loop or extra input HWND. Native diffusion level is fixed in this prototype; STRATA tint/transparency stay themed. Existing blur radius remains available through wallpaper frost.
- Unsupported/disabled cases use wallpaper frost; target failures detach the target and fall back. Close disposes resources. Movement keeps the native material and effects; native-backed windows skip WPF-only content fades.
- Isolated native desktop probe: 14/14 checks passed across repeated enable/disable, movement/resize, hide/show, zero blur and close cleanup. Evidence: artifacts/live-backdrop-probe/results/report.json. This verifies native lifecycle only, not compositor pixels on the active desktop.
- Live visual acceptance pending in a build containing this source: test Light/Dark, theme switches, moving video/windows behind Files and Terminal, drag/resize, slot swaps, focus, fullscreen, monitor/DPI transitions and toggle-off restoration. Do not mark accepted until observed.

### 2026-09-08: Withdraw black-rendering live backdrop

- [~] Owner reports bar and Settings turned black with the experiment enabled, requiring a settings reset. The isolated native target/lifecycle checks did not validate pixels and were insufficient acceptance.
- Removed the Composition target implementation, its UI toggle and animation exceptions. Wallpaper frost is restored. Settings loading clears only a saved experimental opt-in while preserving the rest of the profile; schema 42 is retained.
- This fixes the unsafe experimental routing in source; it does not prove the underlying compositor rendering cause or resolve all original movement jitter. No installed process, user settings, or shell state was changed.

### 2026-09-08: Files-only continuous wallpaper frost trial

- [~] Files now retains the existing WPF wallpaper frost and effects during slot swaps and interactive move/resize. Its existing location/size handlers update the sampled wallpaper region; no native backdrop or desktop capture is introduced. Other app movement policies are unchanged.
- Isolated actual Files window checks: 50/50 passed across Dark/Light, swap/drag/resize, visible frost, retained texture, changing viewbox, rendered nonblack frames and settled state. Inspected full Files and frost-only PNGs from artifacts/files-frost-probe/results. Synthetic patterned wallpaper makes movement/alignment visible. These WPF renders do not establish active-desktop frame pacing, cross-monitor quality, or installed acceptance.
- Rollback is independently published as Preview 20260908-172130 (source 7becdef). This Files-only change is a separate source commit so it can be reverted without restoring the failed Composition prototype.
- Live acceptance pending: test Files swaps and Super+Ctrl drag/resize on the user's actual wallpaper/monitors; observe blur continuity and smoothness. No running shell or user settings were changed by the isolated test.

### 2026-09-08: Restore carried-app animations and extend continuous frost

- [~] Owner accepted the Files-only trial live and requested animated app transfers again, alongside extending continuous wallpaper frost to STRATA surfaces.
- Workspace browsing and carried-app transfers animate again, honoring Reduced Motion/fullscreen. The carried HWND is passed through so right moves enter from the left and left moves enter from the right; other destination apps retain their normal direction.
- Managed STRATA surfaces now retain materials during swaps and interactive movement. Widget frost no longer freezes alignment during expansion/movement; geometry handlers update the cached texture's sampled region. No native backdrop is reintroduced.
- Validation: clean Release build; shared material/motion routing self-tests; isolated Files 50/50 checks and rendered Dark/Light frames confirm visible, aligned frost during swap/drag/resize. Broader widgets/bars/apps and restored cross-workspace animations still require installed visual acceptance. No publication or installation performed for this source change.

### 2026-09-08: Reduce blur-settling delay

- [~] Owner reports roughly half a second for blur to settle. Source inspection identified geometry-only texture changes clearing the existing frost before asynchronous replacement and a 120 ms reveal; final geometry also relied on WPF events rather than explicit post-placement synchronization.
- Preserve the current texture for monitor-size-only changes to the same wallpaper/strength/fit, avoiding clear/reveal while replacement is prepared. Wallpaper/strength/fit changes retain their stale-content guards.
- Synchronize cached frost viewboxes immediately after native animation placement and at transition completion, including widget layers owned by that window. This only updates geometry; it does not decode or blur per frame.
- Validation: clean Release build, 147/147 self-tests including monitor-size texture retention, and 54/54 isolated Files checks including immediate final-alignment recovery in Light/Dark swap/drag cases. Original live timing improvement remains unverified. No publication, install, or settings changes performed.

## 2026-09-08: Fullscreen exit visibility and freeze report
- [~] Corrected a reproduced fullscreen ownership conflict: STRATA borderless fullscreen was also being classified as app-owned native fullscreen. The isolated pre-fix probe failed all five ownership checks; the corrected path passes.
- Fullscreen/expansion commands now settle workspace movement before changing presentation. Restore publishes normal presentation before native mutations and guards reentrant restore/fullscreen detection callbacks. Opening fades finish before fullscreen takes opacity ownership.
- Validation: clean Release build; 148/148 quiet self-tests; 40/40 isolated STRATA Text checks over ten fullscreen/restore cycles across Tiled and Floating; 563/563 isolated desktop checks including new restore cycles and native fullscreen coverage.
- The user's intermittent invisible-window/freeze symptom has not been reproduced in the installed shell. Live confirmation remains pending. Source only; not pushed, published, installed, or restarted.

## 2026-09-08: Blur-enabled Files three-step fullscreen cycle
- [~] User clarified the remaining issue: Expanded -> Fullscreen -> Normal leaves only the active border visible until a workspace round trip. The earlier fullscreen-state change did not resolve installed acceptance.
- Added one deferred, coalesced native frame commit and client/child repaint after a managed app leaves true fullscreen. It runs after material/layout updates, keeps geometry/focus/workspace unchanged, and skips closed, hidden, re-fullscreened or transitioning windows.
- WPF's HwndTarget disables native rendering while position changes are pending and enables it on the completion notification; a visible WPF tree alone therefore does not prove native presentation. References: https://github.com/dotnet/wpf/blob/main/src/Microsoft.DotNet.Wpf/src/PresentationCore/System/Windows/InterOp/HwndTarget.cs and https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-redrawwindow .
- Targeted isolated Files test: six exact three-step cycles with blur in Light/Dark; 36/36 checks including deliberately disabling the native render target and verifying the deferred restore re-enables it. Rendered content captures inspected. Original intermittent live failure not reproduced; user confirmation remains pending.
- Validation completed: clean Release build; 148/148 quiet self-tests; 567/567 isolated desktop checks including the exact Files cycle in Tiled mode. Live confirmation is still required.

## 2026-09-08: Familiar Paint, Snip and Text choosers
- Paint now has a two-row, twenty-color palette, selected-color outline, labeled current color and an Edit colors spectrum button. The custom picker includes hue/saturation/value selection, RGB/hex entry, preview, validation, and OK/Cancel.
- Snip uses six compact markup swatches and More colors, with the same custom-color editor.
- Text uses side-by-side font family/style/point-size lists, editable fields, a sample preview and OK/Cancel. Font size establishes the 100% zoom baseline; Ctrl+0 restores it. Text remains a plain-text document.
- Validation: clean Release build, 148/148 self-tests, 28/28 isolated chooser checks including apply/cancel and invalid values. Light/Dark captures inspected. Source only; not published or installed.

## 2026-09-08: Floating snap placement preview
- Floating title-bar dragging now shows a destination rectangle at side/corner/top snap zones using the existing active-window border renderer, theme accent and glow. Preview and release share work-area/minimum-size target calculation.
- Preview is an owned, nonactivating, click-through surface. It is reused within a zone, retired before changing zones, and cleared on release/cancel, leaving the zone, mode changes, window cancellation, or service disposal. The update timer runs only during a native drag; creation failures suppress retries for that drag.
- Validation: clean Release build, 148/148 self-tests, 14/14 isolated native placement/preview checks (bounds, focus, click-through styles, zone changes, dismissal and tiled exclusion). Outline capture inspected. Physical pointer/installed-shell acceptance remains pending. Not published or installed.

## 2026-09-08: Minimized Floating dock previews
- Removed the preview-card exclusion for minimized windows. Native DWM thumbnails are now attempted for minimized third-party apps too.
- STRATA apps retain a small frozen WPF image immediately before dock/title-bar/Show Desktop minimization; minimized cards use that last frame without restoring or focusing the source. Cache stays in memory, is bounded to 32 images of at most 460x252 pixels, refreshes on subsequent minimization, and clears on window release/close or manager disposal.
- Validation: clean Release build, 148/148 self-tests and 569/569 isolated desktop checks. Minimized-card capture inspected; the app stayed iconic and hidden while the preview was open. Third-party DWM pixel/live installed acceptance remains pending. Source only; not published or installed.

## 2026-09-08: USB drives incorrectly reported non-ejectable
- Fixed a Configuration Manager/SetupAPI property-ID mix-up: CM_Get_DevNode_Registry_Property used 0x0F (physical device object name, REG_SZ) instead of CM_DRP_CAPABILITIES 0x10 (REG_DWORD). The four-byte read failed before checking the removable capability.
- Added named constants and DWORD type/length validation. Existing system-drive, multi-disk, USB-parent boundary, revalidation and Windows veto safeguards remain in force.
- Validation: installed Windows SDK cfgmgr32.h confirms IDs; read-only probes on three present USB nodes returned CR_BUFFER_SMALL (0x1A) for the old query and CR_SUCCESS with a four-byte DWORD for the corrected query. Clean Release build and 148/148 self-tests passed, including capability validation regressions.
- No USB storage device is attached on this host, so physical ejection is not verified. No devices were ejected. Source only; not published or installed.

## 2026-09-08: Fullscreen, dock recovery and desktop customization follow-up
- Fullscreen restoration now explicitly returns the desktop HWND to the bottom after showing it. The regression test includes the actual desktop and verifies native Files/desktop stacking through expanded/fullscreen/center cycles; prior tests checked only app visibility and opacity.
- Work-area publication updates rcWork immediately and notifies other apps without waiting on their UI threads. Changing the replacement-shell rail edge uses one reservation update. A deliberately slow notification receiver confirmed 2 ms publication and received the change.
- Installed logs identified detached dock anchors and reentrant preview closing as Recovery triggers. Preview creation now requires a connected anchor; preview ownership clears before Close, and preview actions close before activating another app.
- Schema 43 adds independently switchable carried-window workspace animations and two bloom scopes. The smaller scope keeps text/graph/icon glow while removing large surface/control/widget bloom. Existing choices and full bloom appearance remain defaults.
- Added destination-monitor workspace number popups; renamed STRATA Text to STRATA Notepad without changing saved launch identities; aligned Wrap; removed decorative app numbers; shared two-tone vector app icons between dock and launcher.
- The keybinding editor can explicitly replace application-launch shortcuts. System/editor/recovery and other custom shortcuts cannot be overwritten; removing a replacement restores the default application action.
- Validation: Release build and 148 self-tests passed; 578 isolated desktop checks passed in a serial run; 29 focused Light/Dark, bloom scope, icon, typography, persistence and notification checks passed. A concurrent work-area test run was invalidated and replaced by the clean serial run. Source-built only; not installed or published. Physical installed fullscreen/driver acceptance remains pending.

## 2026-09-08: Bloom quality Low / High
- Schema 44 adds a live Low/High choice beside bloom scope and strength; High preserves the existing appearance and defaults.
- Low replaces large surface blur with translucent frame strokes and Canvas line-graph blur with two bound noninteractive stroke layers. Small text/vector halos use a tighter blur and bounded caches; returning to High or disabling bloom removes owned layers/caches. Frosting and ordinary shadows stay separate.
- Validation: final Release build succeeded without warnings; 148 self-tests passed; 58 focused Light/Dark, persistence, live graph update, scope, cleanup and ownership checks passed, with captures inspected. Isolated desktop runs passed 581/582 checks, including all new Low/High widget assertions. The embedded player play-pause input probe failed twice and also failed in a High-only control run, so it is not specific to Low bloom; that test limitation remains unresolved.
- Source only; not published, installed or activated. Installed GPU/frame-rate improvement remains unmeasured.

## 2026-09-08: Embedded-player input acceptance follow-up
- Established that the prior bloom-validation failure was test state, not a Low-quality rendering regression: releasing the seek-capture fixture scheduled a normal hover collapse during WebView2 navigation. The next click targeted a collapsed 358x11 area with detail hit testing disabled.
- The independent playback-click fixture now maintains simulated player hover and re-expands the widget before navigation, then clears its pointer override. A new precondition verifies expanded interactive content and actual WPF hit ancestry before sending native pointer messages.
- Validation: Release build and quiet self-tests passed; the isolated desktop suite passed 583/583 checks. The player was 358x246 and received exactly one DOM click; seek capture/release and Spectrum routing also passed. This resolves the previously recorded 581/582 test limitation. No production input behavior changed; physical installed input remains a separate acceptance check. Not published or installed.

## 2026-09-08: Notepad toolbar label consistency
- Font used default Button typography, while the other actions used SignalLabel; Wrap alone centered its text beside the icon. The toolbar now shares a local SignalLabel-based style with consistent vertical centering, including Font and Wrap. Save retains its existing accent color.
- Release build and 29 focused UI checks passed; Light/Dark toolbar captures inspected. Updated the reused probe's stale schema expectation to the current schema constant. Source only; not published or installed.

## 2026-09-08: Handwritten Notes widget
- Notes titles, snippets and editing fields now use Segoe Print with Ink Free / Segoe UI fallbacks. Editing text is 16 px and snippets 14 px for readability; custom title capitalization is preserved. Technical labels and controls keep their existing typography and theme colors.
- Release compilation succeeded; actual expanded Notes widget captures inspected in Dark and Light on an isolated desktop. The visual probe exited successfully and produced both captures, but its wrapper reported the omitted completion JSON; this was a visual inspection, not an acceptance-suite run. Source only; not published or installed.

## 2026-09-08: Manual clock changes replaced after sign-in/reboot
- Report concerns another fresh Windows installation; MAINPC readings do not diagnose that machine. Source review confirmed Set Clock changed the local clock while leaving Windows synchronization enabled. A wrong zone or hardware-clock issue on the affected machine is not ruled out.
- Set Clock now explicitly confirms manual time and configures persistent Windows NoSync before setting the clock. Use Automatic Time restores configured network/domain sources without replacing server addresses. Organization Type policy is respected, and failed clock writes attempt to restore the prior mode. No startup replay of a saved timestamp.
- Validation: Release build and quiet self-tests passed; 583/583 isolated desktop checks passed. Four injected clock-ordering, refusal, rollback and source-mapping checks passed without changing native time. The focused native-settings suite passed 49/50; its separate live Floating keybinding-map assertion failed and was left outside this clock change. Affected-PC privileged apply and reboot/logout acceptance remain pending. Source only, not published or installed; this host clock and synchronization settings were not changed.

## 2026-09-08: High preset matches the running shell
- Captured the persisted profile used by the running 20260908-214919 shell on MAINPC. Updated High to select Low bloom quality, full bloom scope and disabled window/widget shadows; its other effect values already matched. High detection includes these choices plus blur strength/widget opacity.
- Validation: Release build and quiet self-tests passed; a focused read-only comparison matched all 13 preset-controlled effect fields against the running shell profile and verified High/Custom detection. No installed settings changed. Source pending Preview publication.

## 2026-09-08: Notification and confirmation polish
- Removed the circled Explorer-session/failsafe paragraph from the quick power panel. The actual Explorer-session confirmation and recovery behavior remain intact.
- Shared dialogs now use natural title casing, readable UI-font actions/body text, softly highlighted surfaces, tinted status badges and a divided action area. Removed redundant SYSTEM SIGNAL and default tone captions; meaningful custom captions remain. OSDs use clearer label/value columns, accent detail and a correctly sized progress track.
- Release compilation passed. Actual dialog and OSD captures inspected in Light/Dark; 32 focused confirm/cancel and Enter/Escape checks passed across all four tones. No live-shell installation or publication performed.

## 2026-09-08: Remove wallpaper-name banner
- Wallpaper cycling now changes the image without showing a filename OSD. Removed the banner rather than resizing it, per the updated request. Release build passed; source only, not published or installed.

## 2026-09-08: Light/Dark theme-switch freeze
- Installed MAINPC log showed an unresponsive dispatcher during Wallpaper presentation at 22:39:08, followed by forced shell termination. Windows theme sync was enabled. Apply/Restore synchronously broadcast WM_SETTINGCHANGE from the shell dispatcher; SendMessageTimeout grants each top-level receiver its own timeout, and same-queue calls ignore that timeout entirely. This is a confirmed blocking path consistent with the report, not an installed stack-trace diagnosis.
- Windows preference writes and backups retain their original ordering; notifications now use one coalescing worker queue. The native call stays synchronous on that worker to preserve string-pointer lifetime. Rapid changes collapse to a pending notification and idle queues restart safely.
- Validation: clean Release build and quiet self-tests passed. Three regression checks verified dispatcher responsiveness with a blocked receiver, coalescing, and restarting. Native-settings suite retained the separate preexisting Floating keybinding assertion failure. Tests did not mutate Windows theme preferences. Installed Light/Dark acceptance remains pending. Wallpaper-name banner removal remains included in the pending source changes; not published or installed.

## 2026-09-09: Secondary rail misses wallpaper/theme colors
- Found retained brush instances in network/audio indicators and active-workspace borders, plus a fixed cyan selection fill. These relied on a background status timer to catch up after palette replacement. Replaced them with dynamic theme resources and themed selection fill.
- Every rail now refreshes status and material on theme publication and when shown again, including after monitor-specific fullscreen suppression. No workspace geometry or appbar reservation changes were added to the theme handler.
- Validation: Release compilation passed. A private-desktop probe created rails for both actual monitors, stopped their status timers, cycled Light/Dark six times, replaced palette brushes directly and hid/restored the secondary rail. All 49 color/surface/selection checks passed. This addresses a confirmed stale-brush path; the user's intermittent installed issue, particularly if only frosted background pixels are affected, still needs live verification. Source only; not published or installed.

## 2026-09-10: Browser downloads opening Windows File Explorer
- Confirmed two bypasses: downloaded archives used Windows file associations, and Ask Where to Save enabled WebView2's native download UI. Browser Open folder/Reveal already created STRATA Files windows.
- Supported archives now open for browsing in STRATA Files. Download save prompts use its shared save picker, keeping overwrite confirmation/cancellation. Native download dialogs and internal downloads pages redirect to the STRATA panel. File-type picker labels now fall back to their readable display text in selection templates.
- Validation: clean Release build, 148/148 quiet self-tests and 24/24 real WebView2 download checks on a private desktop. Checked Open folder, Reveal, archive browsing, unique filenames, native UI redirection, choosing a Unicode save name and cancellation. Inspected the actual save picker in Light/Dark and archive contents in Files. Evidence: `artifacts/browser-download-probe/results-final/report.json` and PNG captures. Installed 1.0.17 remains unchanged; live acceptance and publication are pending.

- [~] Sphere Browser/widget follow-up: real Browser pages with compact chrome, live internal-tab splitting and independent window restoration, top-rail tabs/full-height content, Floating/Tiled/Sphered selection order, and each monitor's desktop widgets on new tabs with contextual Super + Shift + D. 154 isolated Sphere checks passed; Browser-close comparison reproduces five existing baseline failures. See `docs/SPHERED.md`; installed acceptance remains pending.

- [~] Sphere follow-up: restored wide home search, removed tab-title popup interference, bypassed app position rewrites during pane placement, and repaired app-owned close focus handoffs. Titled Browser successor and semantic shortcut checks replace invalid fixture assumptions. 210 combined Browser/Sphere checks passed; live Steam/hover acceptance remains pending.
