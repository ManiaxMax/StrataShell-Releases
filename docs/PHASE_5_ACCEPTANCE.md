# Phase 5 — code, fresh-install defaults and documentation

September 5, 2026. The owner approved this pass after the pre–Phase 5 implementation and explicitly limited default changes to brand-new release installs. [Current release/testing status](STATUS.md) preserves outstanding physical checks.

## Changes

- The installer template retains Balanced quality and Stratanow selects Floating, the approved eight-widget arrangement, and Clock/YouTube expansion locks. Personal notes, weather location, accounts, device IDs and startup choices are not copied. Schema 40 and runtime settings defaults/migrations are unchanged.
- Portable installation now seeds the same safe template after payload validation only when a settings file does not exist. Setup's existing non-overwrite seeding also preserves a profile created during preparation. Only explicit Reset Settings may replace it.
- Code review added cancellation to deferred tray-window restoration, matching the existing native-menu cancellation contract. A newer panel action cannot be followed by a stale window activation.
- Previous sound dropdown/focus, tray runtime dependency and widget startup fixes are included in the reviewed Preview working tree.
- README branding uses a transparent vector of the canonical mark at 720 pixels instead of 640. Light/dark wordmark colors preserve readability. Existing launcher icons are unchanged.
- Core documentation is updated for current native settings, first-party apps, Floating/Tiled behavior, release channels and verified limits. Public documentation contains no application source or private test artifacts.

- The ordinary-maximize regression exposed missing native work-area publication without Explorer. Replacement-shell rails now own per-monitor, session-only reservations and release them for fullscreen/shutdown. Custom-chrome maximized windows are no longer mistaken for fullscreen games. Successful placement resets the bounded correction budget, and physical-pixel reservations are no longer clipped at 120 pixels.

## Verification

The focused fresh-install/preservation checks pass **22/22** for Setup and portable seeding, including byte-identical retention of existing profiles. The runner is `scripts/Test-StrataReleaseDefaults.ps1`; its local report is `artifacts/phase5-defaults/report.json`.

Release builds for Shell, Launcher, Maintenance and Setup passed with no warnings or errors. The complete application suite passed **327/327**; the expanded desktop suite passed **555/555**, including direct Windows work-area publication, repeated native/custom-chrome maximize, top/bottom edges, 176-pixel reservations and restoration. Quiet self-test passed **144/144**. An earlier self-test run overlapping the application suite timed out in its ConPTY probe; a sequential rerun passed. This is recorded as test scheduling sensitivity, not an installed product failure or a reason to weaken the check.

Reports remain local: `artifacts/phase5-apps/report.json`, `artifacts/phase5-desktop-verified/report.json`, `artifacts/phase5-self-test.json` and the defaults report above. Repository documentation is reviewed separately from binary publication; the refresh does not advance the Stable application source or install a new release.

## Deployment boundary

This pass changes source and documentation. It does not promote a Stable binary, install a new shell, restart the session or overwrite the current user's settings. Before Phase 5, release `20260905-110411` was confirmed running after the user's restart; the later sound polish and Phase 5 changes are not part of that installed release.

## Installed Preview follow-up

Later on September 5, Preview `20260905-135348` was confirmed running after an intervening publication/install. Its shell DLL matches the candidate ZIP whose manifest identifies source commit `e3588b99ca7a36b086b54a2e2c2226466af03504`. That commit includes the Phase 5 implementation. The installed template contains Balanced, Floating, the approved widget layout and Clock/YouTube locks. Both running processes load .NET 9.0.19; the installation self-test completed successfully with 144 checks. This supersedes the earlier deployment boundary above. It does not establish physical acceptance of every feature or a migration to .NET 10.

The owner then reported a recovery dialog after opening desktop apps. The installed log identifies `InvalidOperationException: Collection was modified; enumeration operation may not execute` in `NotificationAreaService.Snapshot`, called by the rail refresh. App identity lookup enumerated ManagedShell's live registration collection while native/COM activity could reenter its mutation path.

The local correction gives each notification host its own collection and publishes a stable membership array inside each collection-change callback, before native work or subscriber notifications. Readers keep that array throughout grouping. Later refreshes observe additions, removal, replacement, movement, reset and visibility changes. Disposal clears the published membership; recovery handling remains intact.

The focused isolated runner passes **52/52**, including a deterministic reproduction of the old live-enumerator exception, survival of the equivalent snapshot read, and 500 reads during 600 collection changes. Release build passes without warnings/errors; quiet self-test passes **144/144**. Two preceding focused runs failed the existing click-only fallback timing assertion; the installed baseline passed its 44 checks, and the diagnostics-enhanced final run passed all 52 without changing fallback behavior. The extra timing/input diagnostics remain in that test, and physical tray acceptance is still open.

Local evidence: `artifacts/tray-recovery/diagnostic/report.json`, `self-test.json` and `installed-verification.json` in that same directory. This correction is source-built only: no release was published, installed or restarted during the recovery investigation. Completion requires deployment and repeating the owner's app-opening sequence on the active desktop.

## Power, runtime and stability follow-up

The owner subsequently approved the .NET 10 migration, native power controls, battery history, Windows Update bridge, widget/scroll fixes, uninstall cleanup and investigation of another hang. See [the current implementation and evidence](POWER_RUNTIME_FOLLOWUP.md). This is a later local candidate; the installed .NET 9 statement above records the last verified deployment, not the current source target.
