# Installed STRATA runtime interaction and performance audit

Date: 2026-08-31
Test system: primary 2560 x 1440 single-monitor workstation
Audit type: live installed-runtime observation, source-side documentation only

## Build identity and evidence boundary

This audit exercised the STRATA Shell process that was already installed and running:

- Installed release directory: %LOCALAPPDATA%\Programs\StrataShell\releases\20260831-105006
- Running product version: 1.0.6+4bd291947041043eb55da4ea611e427d3d0293ff
- Running file version: 1.0.6.0
- Runtime start: 2026-08-31 11:57:14 EDT
- Runtime remained responsive at the end of the audit.

The source checkout was not used to launch the tested shell:

- Source checkout: C:\Projects\StrataShell
- Documentation branch: docs/runtime-qa-installed-2026-08-31
- Source baseline: bbc6d61ef4e511367f18d84f4a9011f491acb125

The installed binary and source baseline are different revisions. Every observed defect below is therefore confirmed only for the installed 1.0.6 runtime. A finding must be reproduced against a source-built Preview before changing code, because newer source work may already affect the same path. No application code was changed during this audit.

## Corrective source status — 2026-08-31

A subsequent corrective pass is in progress on `fix/runtime-presentation-visuals-2026-08-31`, based on source `bbc6d61ef4e511367f18d84f4a9011f491acb125`. It does not replace or restart the installed `20260831-105006` shell used for the observations above.

Implemented in source:

- widget expansion freezes and realigns only the moving widget's cached frost layer; unrelated widgets retain their blur instead of participating in a window-wide frost suspension;
- opening fades are settled before a workspace, slot, or desktop-view transition captures its opacity baseline, preventing a temporary zero-alpha opening frame from becoming the restored state;
- transition completion reapplies each window's configured transparent or opaque state after restoring its captured baseline;
- the active-border target gate rejects hidden, DWM-cloaked, and constant-alpha-zero windows;
- the overlay performs the same presented-state check while tracking, is positioned invisibly first, and appears only after stable bounds have survived the settle gate;
- active-border state is removed during workspace, slot/layout, and floating-drag motion, then delayed until geometry settles;
- routine identical active-border diagnostics no longer repeat every refresh; and
- periodic layout reconciliation no longer sends frame-changing placement calls to already-settled tiled windows.

Source verification completed after these changes:

- all four Release projects build with zero warnings and zero errors;
- the deterministic shell suite passes 82/82 checks, including the scoped widget-frost freeze, exclusive opacity lease, presented-state border gate, invisible-first overlay placement, and settled tiled-geometry policy; and
- the isolated critical UI suite exits successfully and passes STRATA Files, Settings, Terminal, Text, Snip, all seven widgets, hover transitions, persistent AI/YouTube expansion, widget live-theme updates, and first-party glass checks.

This is source evidence, not installed-runtime acceptance. The corrective Preview still requires the matrix below, especially overflow into another workspace, rapid desktop-view changes, populated YouTube hover, and blur/bloom combinations. Bright native-surface contrast, app-specific refusal of DWM caption colors, and resource-soak limits remain separate follow-ups rather than being changed through an unverified per-process opacity or title-bar allowlist.

## Acceptance goal

The shell should remain smooth, beautiful, and deterministic while native and first-party apps open, close, tile, move, change workspaces, change desktop modes, overlap, float, and use blur or bloom. A transition is acceptable only when:

- the complete app surface remains visible unless policy intentionally occludes it;
- focus transfers to the intended survivor or destination;
- geometry, opacity, blur, bloom, and the active-window signal settle atomically;
- no intermediate border line, stale overlay, flicker, jump, ghost, or delayed reveal remains;
- static widgets retain their intended material while only the actively moving surface is simplified;
- frame pacing remains responsive and resource ownership stays bounded.

## Runtime configuration

The installed shell was tested with the user's active settings, including:

- first-party surface blur enabled;
- widget blur enabled;
- bloom strength 0.40;
- application transparency enabled at approximately 80 percent opacity;
- reduced motion disabled;
- high contrast disabled;
- window shadows disabled;
- widget shadows disabled;
- five workspaces;
- widget hover expansion enabled with a 90 ms delay;
- UI and widget scale at 1.00;
- surround thickness 7 px.

First-party surface blur was temporarily disabled through the installed Settings app for an A/B observation, then restored. The settings file was checked afterward and the original values were restored.

## Test matrix and result

| Scenario | Result | Runtime observation |
|---|---|---|
| ChatGPT plus native Notepad, top/bottom stack | Pass | Equal-width tiling, correct top/bottom geometry, correct active signal, and correct full-height reflow after Notepad closed. |
| Native app overflow from a full workspace | Reproduced, critical | Paint was assigned to the next workspace, but its client and chrome became invisible after the slide while a large accent outline remained visible. |
| Recovery after the invisible Paint state | Reproduced | Direct activation did not restore Paint. Leaving and re-entering its workspace restored the full window, proving the process remained alive and the failure was presentation state rather than process death. |
| Calculator first presentation on an empty workspace | Reproduced, intermittent | The first rail observation showed an empty workspace while direct app capture showed Calculator alive. A later rail observation showed the complete full-stage app. |
| 7-Zip first presentation | Reproduced, intermittent | The first frame retained the previous workspace content; the new workspace/app appeared roughly 300 ms later and then settled correctly. |
| Notepad++ joining 7-Zip | Reproduced, intermittent | The existing full-stage app remained alone for the first frames; Notepad++ appeared later and the final top/bottom stack was correct. |
| Repeated focus changes within a settled native stack | Pass for this route | The complete active outline appeared promptly. The reported vertical-line-to-full-frame widening was not reproduced during focus-only activation. |
| Settings opening on an empty workspace | Partial | The earliest capturable frame was centered and fully composed. The reported pre-presentation expansion/flicker was not reproduced, but it may occur before the capture API can observe the window. |
| First-party blur off/on A/B | Pass for control function | Disabling first-party blur produced a noticeably sharper and busier transparent Settings surface. Re-enabling it restored wallpaper softening. Widget blur stayed enabled. |
| Screensaver idle appearance and dismissal | Partial | The themed screensaver appeared, rendered a centered logo/effects scene, and dismissed with Escape. Sustained frame rate was not measured. |
| YouTube populated-result hover | Not executed | The embedded search field did not accept the noninvasive programmatic input path, so the reported delayed center-window disappearance over a video result was not assigned a pass or failure. |
| Super-key desktop, move, float, and fullscreen routes | Not executed | The safe desktop-control harness does not synthesize Windows/Super shortcuts. These require manual installed-build acceptance. |
| Multi-monitor routing | Not executed | Only one display was connected. |

## Confirmed findings

### QA-01 — Transition can leave a live native window fully invisible

Severity: Critical
Reproduced with: Paint overflowing from a full workspace to the next workspace

Observed sequence:

1. The shell selected the correct destination workspace and began the expected right-to-left slide.
2. The first transition frame contained the moving window.
3. By approximately 700 ms, Paint's client and native chrome were no longer visible.
4. The cyan active-window outline remained at a large valid-looking rectangle.
5. Direct activation did not restore the surface.
6. A workspace leave/re-entry restored Paint immediately.

This is the strongest evidence from the audit. Window ownership and placement survived, while one or more presentation properties did not complete or restore. The border overlay continued to treat the HWND as active and valid even though the represented surface was not presented.

Likely source owners to inspect:

- src/StrataShell/Services/WindowManagerService.cs
- src/StrataShell/Services/WorkspaceSlideTransition.cs
- src/StrataShell/Services/ManagedWindowAnimationVisualPolicy.cs
- src/StrataShell/Interop/WindowEffects.cs
- src/StrataShell/Views/ActiveWindowBorderOverlay.cs

### QA-02 — First presentation is not atomic across workspace and app activation

Severity: High
Reproduced with: Calculator, 7-Zip, and Notepad++

Three independent native-app cases showed a short-lived mismatch between workspace state and visible app state:

- an apparently empty rail workspace while Calculator was already alive;
- the prior Calculator workspace remaining visible during the first 7-Zip frame;
- a full-height 7-Zip surface remaining alone before Notepad++ appeared in the final stack.

Final geometry was correct in these cases, so the defect is concentrated around focus, visibility, capture, or transition readiness rather than steady-state tiling.

### QA-03 — Active-border state can outlive the surface it represents

Severity: High

During the Paint failure, the border overlay remained visible and continued to refresh while the application surface was invisible. In the last 5,000 log lines:

- 3,283 lines were Active theme border messages;
- 209 lines referenced the Paint HWND;
- zero Error and zero Warning lines were present in that sample.

The Paint border message repeated approximately every 1.5 seconds. This both proves that the shell still considered the HWND valid and creates high-volume state logging that can obscure useful diagnostics and add avoidable I/O/allocation pressure. src/StrataShell/Services/WindowManagerService.cs currently emits this message from the active-border update path near line 4091.

### QA-04 — Transparent bright native surfaces lose hierarchy

Severity: Medium

The white 7-Zip content surface became visibly washed out under the approximately 80 percent global application opacity. The wallpaper remained attractive, but labels, rows, and controls lost separation. This is a material/readability issue rather than a geometry defect.

Recommended policy: keep user-selected translucency for compatible dark surfaces, but apply a contrast floor or per-window compatibility rule to bright native surfaces. Do not silently force all windows opaque; preserve the user's transparent visual direction while keeping text readable.

### QA-05 — Some native title bars still do not follow STRATA theme color

Severity: Medium

Native Notepad used a pale/light-blue title bar while the surrounding STRATA palette was dark and cyan. This matches the previously reported native title-bar mismatch and should be checked separately from client transparency.

### QA-06 — Runtime resource growth needs a controlled soak, not a leak claim

Severity: Medium, diagnostic follow-up

The final five-second static sample showed:

- 0.422 CPU seconds over five wall-clock seconds, equivalent to approximately 8.4 percent of one logical core;
- working set 543.7 MB;
- private memory 609.0 MB;
- 43 threads;
- 1,305 handles;
- Responding = true.

Across approximately 72 minutes that included active launches, transitions, capture, Settings changes, and cleanup, working set increased by about 51.7 MB, handles by 157, and threads by 8. Private bytes were not monotonic: approximately 629 MB, then 652 MB, then 609 MB. This does not prove a leak. It does justify a repeatable idle/interaction soak with ownership snapshots for overlays, timers, event subscriptions, captures, and native handles.

## Areas not reproduced

- A vertical line widening into the active-window frame was not seen during ordinary focus switching after windows were settled. It remains plausible specifically during open, move, workspace, or desktop-view transitions.
- The Settings startup expansion/flicker was not captured through the tested top-bar launch. The capture API may begin after WPF's earliest frames, so this result is not a dismissal of the report.
- The exact YouTube populated-result hover disappearance was not exercised, so no conclusion was assigned.
- The widget-expansion path that reportedly disables blur across all widgets was not conclusively exercised.

## Recommended implementation direction

These are source-review hypotheses, not claims that the current source still contains the installed defect.

1. Use one cancellable presentation transaction per HWND.
   - Acquire ownership before changing geometry, opacity, visibility, blur, bloom, capture, or focus.
   - Cancel or supersede the previous transaction explicitly.
   - Restore every temporary native and WPF property in a finally path.
   - Reject late callbacks from an obsolete transaction token.

2. Make presentation order explicit.
   - Hide the active border.
   - Prepare destination geometry and the capture-free first-party live composite.
   - Commit window placement.
   - Confirm that the target surface is visible and presentation-ready.
   - Reveal/fade the content.
   - Transfer focus.
   - After a stable settle interval, position and reveal the complete active border.

3. Separate native and first-party restoration.
   - Native HWND opacity/layering restoration and WPF Window opacity/effect restoration should not share implicit assumptions.
   - Record the exact pre-transition state and restore it once, rather than recomputing from possibly stale focus/workspace state.

4. Bind the active border to presented state, not only HWND validity.
   - Do not display a frame around a hidden, cloaked, zero-opacity, transition-owned, or nonsettled surface.
   - Move routine border diagnostics to state-change logging; avoid periodic identical messages.

5. Coalesce focus and layout reconciliation.
   - The intended destination window should remain authoritative until the transition settles.
   - Raw foreground HWND reports from Windows should not overwrite an active handoff transaction.

6. Preserve material continuity during interaction.
   - Suspend expensive blur/bloom only on the first-party surface currently moving or resizing.
   - Keep static widgets and unrelated windows visually stable.
   - Restore the moving surface's material only after its geometry is stable.

7. Instrument before further visual tuning.
   - Record WPF RenderCapability tier and whether software rendering is active.
   - Capture ETW/PresentMon frame timing for shell composition, DWM, and first-party apps.
   - Add transaction IDs and single-shot phase markers for prepare, place, reveal, focus, border, complete, cancel, and restore.
   - Track live overlay count, event subscriptions, timers, capture bitmaps, threads, and handles during a soak.

## Required acceptance matrix for a corrective Preview

Run at least 50 cycles of every high-risk route:

| Dimension | Required coverage |
|---|---|
| App combinations | native/native, native/first-party, first-party/first-party |
| Occupancy | empty workspace, one app, two apps, overflow to next available workspace |
| Operations | open, close, focus, swap, move between workspaces, desktop-view change, tile/float, drag/resize, soft fullscreen, hard fullscreen |
| Effects | blur off/bloom off, blur on/bloom off, blur off/bloom on, blur on/bloom on |
| Materials | dark native surface, bright native surface, transparent first-party surface |
| Displays | 1440p primary, ASUS ScreenPad/asymmetric secondary display, different refresh rates and DPI |
| Widgets | idle, expand/collapse, AI Command active, YouTube populated and playing |

For every cycle, assert:

- no client or chrome becomes unintentionally invisible;
- no old workspace or old app remains in the destination frame;
- final geometry exactly matches the selected layout;
- exactly one intended window owns focus;
- opacity and material return to the configured values;
- the active border is absent in flight and complete only after settle;
- the previous location fades out and destination fades in without a blank or oversized text frame;
- closing one app never closes a second app and immediately promotes the correct survivor;
- CPU, working set, handles, threads, overlays, timers, and subscriptions remain within defined bounds.

## Recommended fix order

1. Prevent invisible live windows and guarantee presentation-property restoration.
2. Make workspace/open/close/focus transitions atomic.
3. Gate the active border on settled visible state and remove repeated state logging.
4. Measure and correct first-party/visual-effect frame pacing.
5. Preserve static widget material during expansion and test populated YouTube hover.
6. Add native bright-surface contrast compatibility and native title-bar theme policy.
7. Complete single- and multi-monitor soak acceptance.

## End-of-audit restoration

- First-party and widget blur were returned to enabled.
- All other inspected settings matched the original settings values.
- Audit-only Paint, Calculator, 7-Zip, and blank Notepad windows were closed.
- The pre-existing unsaved Notepad++ document was preserved and left open.
- ChatGPT was returned to workspace 1.
- The running shell was not restarted, replaced, or launched from source.
- Explorer and shell activation policy were not changed.
- No source implementation was performed; only this QA documentation and its backlog reference were added.
