# Phase 2: laptop responsiveness acceptance

Date: 2026-09-04. Scope: audit P2-P7, plus the reported Zenbook browser freeze during cross-screen movement.

**Status: implemented and locally verified; laptop acceptance remains open.** This is a local test build, not a published Preview release. The running installation remains Phase 1.

## What changed

| Area | Implementation | Local evidence / remaining limit |
| --- | --- | --- |
| P2: effective performance policy | Battery / energy-saver state now reaches the rendering policy. Automatic low effects suppress actual bloom and both surface/widget frost. Saved preferences survive; manual High remains an override. Sustained missed animation budgets can cap later optional animation cadence at 60 Hz. | Deterministic policy checks. AC/battery transitions and visual fidelity still need physical laptop testing. |
| P3: repeated UI work | Rails and desktop metrics consume one shared snapshot service. CPU/memory, network/audio, and process metadata run on separate single-flight background lanes. Expensive process/device discovery is demand-driven. Task Manager renders its selected pane, skips hidden/minimized sampling, warms specifications off the UI thread, and caches GPU metadata for 30 seconds. | Build and shared-policy checks; no target-machine CPU improvement percentage claimed. User audio actions update the cached display immediately. |
| P4: animation pacing | Both dispatcher-driven workspace and slot-swap transitions honor the selected frame cap and deduplicate composition callbacks. Elapsed-time pacing preserves the final geometry. | 30/60/native caps tested against 60/144/165 Hz callbacks, duplicate callbacks, and exactly-once final positions. |
| P5: blocking diagnostics and compositor wait | A bounded asynchronous log writer retains 1,024 pending messages and rotates three files of approximately 4 MiB each. Credential-shaped values are redacted. DwmFlush runs on one background worker after border retirement, instead of blocking shell input. | 20,000-message logging fixture; final-message flush and rotation; 1,000 duplicate compositor requests rejected while a synthetic drain is blocked. Border retirement still needs live movement checks. |
| P6: wallpapers and images | Wallpaper cache: 96 MiB. Frost cache: 64 MiB. Shared image decode limit: two workers, with obsolete queued requests canceled. Frost rendering remains on one dedicated STA. Image Viewer ignores obsolete loads and enumerates folders off-thread. | Cache eviction/coalescing/cancellation, real WIC decoding, exact alpha pixels, and real offscreen frost rendering tested. Budgets cover retained cache pixels, not total process memory or active image references. Running WIC work cannot be forcibly canceled safely. |
| P6: Paint | Tile deltas replace full-canvas undo snapshots. History has a 64 MiB delta budget and at most 30 changes, plus a baseline canvas. Stroke uploads cover changed rectangles. Fill, clearing, history commits, Undo/Redo, import and encoding run in the background; editing is guarded during those operations. Save/Discard/Cancel remains available. Ctrl+Shift+Z now reaches Redo. | Exact sparse/dense undo/redo, redo invalidation, dirty uploads, and 100 irregular fills against an independent reference. An actual unshown Paint window also filled, saved/reopened, undid/redid exact pixels and rejected a canceled worker result. Physical drawing and save-dialog workflows remain manual checks. |
| P7: inactive widgets and browser tabs | Desktops share one spectrum capture/FFT per selected source. Hidden/occluded widgets release their subscriptions; each desktop permits only one pending spectrum callback. Background tabs are created lazily and hidden initialized tabs request WebView2 Low memory without suspending scripts, media or network connections. | Browser memory policy tested. Real audio device switching, multi-display widget behavior, background calls and WebView2 lifecycle remain manual acceptance. |
| Browser movement and recovery | Full reconciliation is deferred during native and STRATA-driven floating drags. Browser actions that can create/close windows are posted after WebView2 callbacks. The full-window bloom shader is removed from above hosted web content; chrome retains bloom. History writes are coalesced and atomic; download-folder IO uses a deferral. Browser failures show explicit reload/wait choices. | Source review and offscreen recovery-pane layout. No browser auto-reload or global GPU acceleration override. Zenbook root cause is not yet proven. |

Oversized images are rejected explicitly rather than silently reducing export resolution: Paint supports up to 16,777,216 pixels, Image Viewer up to 33,554,432 pixels, with an 8,192-pixel maximum side for either app. Wallpaper previews may be scaled within their decode budget.

For the tested thirty sparse strokes on a 4096 × 4096 canvas, retained delta data is 491,520 bytes. Including the 64 MiB baseline, the history pixel storage is about 64.47 MiB, versus 1,920 MiB for thirty former full-canvas snapshots. This excludes the active canvas, WPF bitmap, transient buffers and object overhead; it is not a whole-application memory measurement.

## Build and verification

- Branch: perf/phase-2-laptop, based on Phase 1 source ea4294f0e2ee344ea5e653261d13d325bc7ce0ed. Phase 2 changes remain uncommitted.
- Local package ID: **20260904-223446**.
- Artifact: StrataShell-20260904-223446.zip (`artifacts/phase2-test-bundle/StrataShell-20260904-223446.zip`, local evidence), 98,940,538 bytes. Self-contained shell/launcher bundle with installation scripts; no installation was executed.
- SHA-256: **85E1B2A994D4C94830D27EFD47F2DEDF8604025B2D19C121E1BC17C2A32B09CF**.
- Release build: zero warnings and errors.
- Standard packaged suite: **124 checks passed**, including 18 Phase 2 checks and the existing 106. The independently extracted package with opt-in layout fixtures passed **125 checks**.
- Independently extracted the final ZIP and reran its own executable's quiet suite with offscreen layout fixtures: exit 0, 125/125 passed. Its embedded trust key also verified the archive signature and inventory.
- Tests exercise temporary data, unshown WPF windows, and offscreen render targets. No second live shell or actual browser engine was launched by these fixtures.
- Inspected actual Paint and browser recovery controls at 100% and 150% raster scale. These fixtures do not prove HWND content composition, live per-monitor DPI changes, display-driver behavior or physical input.
- Local evidence: package verification (`artifacts/phase2-performance/package-verification.json`, local evidence), standard packaged checks (`artifacts/phase2-performance/packaged-standard-self-test.json`, local evidence), extracted package and layout checks (`artifacts/phase2-performance/packaged-self-test.json`, local evidence), visual fixtures (`artifacts/phase2-performance/visuals`, local evidence).

The running Phase 1 executable remains under the installed releases/20260904-214214 directory. No sign-out, restart, installation, Explorer switch, shell-policy change, driver change, commit, push or publication occurred during Phase 2. Update-package signing uses the existing Phase 1 trust key; Windows Authenticode signing remains unchanged.

## Measurements and what they do not prove

This host has an RTX 4070, a 2560 × 1440 165 Hz display and virtual display adapters. It cannot establish performance on the reported MX450 or Iris Xe systems.

Windows Performance Recorder CPU/GPU tracing could not start: policy error 0xc5585011. No profiling policy was changed. The new [read-only capture script](https://github.com/ManiaxMax/StrataShell/blob/preview/scripts/Measure-StrataResponsiveness.ps1) samples STRATA and its WebView2 descendants, CPU, private/working-set memory, handles, threads and a bounded WM_NULL response probe. It does not export command lines, page URLs, window titles or usernames.

A twenty-second Phase 1 capture of this host's current session recorded 20 samples for the main shell, about 709.85 MiB average private memory, a 0.76 ms p95 / 6.49 ms maximum UI probe response and no probe timeouts. This was not a controlled movement workload or a laptop benchmark. WM_NULL response time is not presentation/frame latency. There is no comparable running Phase 2 capture yet.

The new shell monitor logs dispatcher delays above 250 ms, with one outstanding probe. A separate warning records a dispatcher still blocked after one second. Browser process failures include failure kind, runtime version, DPI and browser size, without page URLs. These signals help distinguish a dispatcher stall from browser/driver failure; they do not identify every GPU stall.

## Laptop acceptance: user test after switching to this build

Keep the same wallpaper, effects, power mode, display arrangement, refresh rate and scaling for before/after comparisons. Capture both the previous build and this build for sixty seconds while performing the same movements:

~~~powershell
./scripts/Measure-StrataResponsiveness.ps1 -Seconds 60 -Label phase1-browser-cross-screen
./scripts/Measure-StrataResponsiveness.ps1 -Seconds 60 -Label phase2-browser-cross-screen
~~~

1. **Zenbook, secondary display enabled:** repeatedly move Browser between screens using the title bar and STRATA's own floating-window movement route. Include rapid reversals, resizing, and moving while a page loads. Observe the whole shell and another open app.
2. **Zenbook, secondary display disabled:** repeat the same workload on the available displays. Include the original 1920 × 1080 setup.
3. **Iris Xe:** repeat at 1920 × 1200 / 60 Hz. Check 100%, 125% and 150% scaling where supported, mismatched scaling between screens, and a secondary display positioned to the left or below.
4. **Browser continuity:** switch among several tabs, play media, keep a call active in a background tab, open/close tabs during loading, and test downloads. Reload/wait choices must remain usable after a recoverable page failure. Unsaved page work must never be auto-reloaded.
5. **Shell visuals:** compare borders, bloom, transparency and frost before/during/after movements. Check final window geometry, clicks, focus, workspaces, fullscreen, display unplug/reconnect and sleep/resume. On AC/battery transitions, Automatic low effects should apply and restore; manual High should remain High.
6. **Apps/widgets:** use Paint brush/shapes/fill/Undo/Redo, cancel a close with unsaved work, save/reopen PNG and JPEG, close during import/fill, and rapidly change Image Viewer files. Confirm multiple desktops do not cause duplicate audio capture, and hidden widgets resume correctly. Timer alerts and media playback must continue.
7. **Sustained use:** repeat for at least fifteen minutes, watching memory/handle trends and responsiveness. Collect the capture and relevant new dispatcher/browser/compositor log entries if the freeze persists.

Acceptance requires no whole-shell halt in the reported sequence, correct final geometry/input, no media/call interruption or save regression, and no sustained memory/handle growth under the repeated workload. Quantified laptop improvements and the exact freeze cause remain unverified until these tests run.

## Primary references

- [WebView2 threading and reentrancy](https://learn.microsoft.com/en-us/microsoft-edge/webview2/concepts/threading-model): callbacks must finish before nested message-loop work.
- [WebView2 performance guidance](https://learn.microsoft.com/en-us/microsoft-edge/webview2/concepts/performance): share environments, defer creation and use supported memory controls.
- [DwmFlush contract](https://learn.microsoft.com/en-us/windows/win32/api/dwmapi/nf-dwmapi-dwmflush): a blocking wait for the calling application's queued DirectX work.
