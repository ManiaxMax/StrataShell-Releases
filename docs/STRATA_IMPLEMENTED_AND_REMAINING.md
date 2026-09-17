# STRATA — completed work and current acceptance

Updated September 5, 2026. Source: `C:\Projects\StrataShell`.

The latest .NET 10, power, hardware, uninstall, settings-scroll and stability work is recorded in [Power and runtime follow-up](POWER_RUNTIME_FOLLOWUP.md). Earlier source fixes, packaged verification, deployment state and physical checks are maintained in [Follow-up acceptance](FOLLOWUP_ACCEPTANCE.md). The native settings, grouped bar/window, browser-toolbar and synchronized wallpaper work is described in [Native settings acceptance](NATIVE_SETTINGS_ACCEPTANCE.md).

At the start of this follow-up, the running release was `20260905-102604`. Earlier release-state statements below are historical. Phase 5 is now active; source and observed physical acceptance are separate milestones.

The [native wireless follow-up](NATIVE_WIRELESS_ACCEPTANCE.md) adds streaming Bluetooth discovery and PIN pairing, Wi-Fi disconnect/forget/radio controls, a working network click menu and a Bluetooth tray button. Routine Bluetooth and Wi-Fi controls no longer require starting Explorer. Physical pairing and network/radio changes remain to be tested with the candidate installed.

The old agenda has been retired. The [previous detailed inventory](archive/STRATA_INVENTORY_BEFORE_CLEANUP_2026-09-05.md) and [previous backlog](archive/STRATA_BACKLOG_BEFORE_CLEANUP_2026-09-05.md) preserve the history, including earlier completed features, limitations, and evidence. Their unfinished items are not automatically part of the new backlog. Laptop-specific investigations are removed from active scope. Publisher signing is deferred and is not a prerequisite for this pass.

## Completed in this cleanup

| Area | Changes |
|---|---|
| Settings review | Reviewed all 16 distinct pages, including both window modes where applicable, in compact Dark and wider Light layouts. Improved wrapping, descriptions, shortcut labels and hardware-page ordering. Primary network, Bluetooth and audio controls now precede Windows/Explorer fallbacks. Empty imported-taskbar sections are hidden. Duplicate Floating dock-height controls are consolidated in Bar settings. |
| Retired settings | Removed the CPU bar module and its settings. Removed the power visibility setting, inert Bluetooth bar toggle, unused inactive-opacity and shell-spring fields, and nonfunctional Do Not Disturb control. Timer notification wording now describes what that setting actually does. Rendering presets retain their functioning options. |
| Defaults and migration | Schema 39 removes retired fields when settings are rewritten. Existing customized dimensions and unrelated hotkey remaps are preserved. Bar-size reset now resets only size and spacing. Shipped defaults and documentation agree with the source. |
| Dock and shortcuts | The STRATA dock icon opens Applications. The tiled Command launcher remains separate. Super + Esc is removed, including retired built-in remaps. The power button is always available. The catalog contains 76 unique built-in shortcuts: 73 active in Tiled view and 71 in Floating view. |
| Task Manager | Super + T now changes both the main material and nested panels; solid cards no longer defeat transparent mode. The Users page, navigation and unused model are removed. |
| Active outlines | The outline is now a native owned window of its application, instead of an independent always-on-top surface. STRATA also checks overlapping windows above the target. This corrects the Task Manager outline appearing over another app and preserves existing file-picker/modal safeguards. |
| Widget sizing | Expanded cards retain usable height in crowded columns. Columns scroll, and expanded cards are brought into view. Moving a widget clears pending hover/old-column ownership before restoring its lock state. |
| Widget input and polish | Both columns refresh hosted-browser input geometry on scrolling and expansion. Ctrl-drag supports edge scrolling and releases capture on cancellation. Weather condition text is bounded and has a full tooltip. Widget settings and timer alerts respect reduced motion. Existing Notes edit/page behavior is retained. |
| Animation reliability | Native open/close fades follow composition frames and the effective frame-rate policy. Interrupted transitions cancel stale completion actions and restore their original transforms. Hidden desktops have a bounded completion fallback. Reduced-motion paths avoid decorative fades. |
| Tray applications | Rows send native single-click actions; Open sends one selection to version-4 publishers and a double-click to older publishers; Menu and right-click request the application's own menu. STRATA releases popup capture before the app responds and retains the icon's screen position. The Explorer-independent host answers taskbar-presence queries before clients register their icons and follows the primary top/bottom bar geometry. Removed process-restart and force-kill fallbacks. |
| Settings finishing details | Network lists omit addressless Windows filter layers, use readable adapter types, and show the correct Wi-Fi glyph. Long action titles wrap. |

## Earlier fixes retained and rechecked

| Reported issue | Local result |
|---|---|
| Text and Snip entering Recovery after Discard | Deferred close avoids re-entering WPF's active close event. Save, Cancel and Discard regressions pass. |
| Maximized ChatGPT/external apps behind the bar in Floating view | Ordinary maximization uses STRATA's reserved work area while preserving restore geometry. True fullscreen remains a separate path. |
| Active app outline over a save picker | Modal/owned-window suppression is retained, with the stronger native outline ownership described above. |
| Tiling workspace/monitor selection changing unexpectedly | Existing monitor-selection corrections are retained; repeated empty/occupied workspace and monitor sequences pass in the desktop suite. Installed physical acceptance remains separate. |

## Earlier phases and app inventory

The previous inventory records the detailed implementation and evidence for Phases 1–4. That baseline includes browser extension/update/file safety, protected saved passwords, background sampling and rendering policy, bounded caches and image work, display/workspace and fullscreen handling, shared app materials, accessibility names and toolbars, and actual Task Manager hardware counters. This cleanup preserves that baseline; it is not a new security audit or a claim that every historic limitation was implemented.

All ten first-party surfaces are included in the app regression run: Text, Files/picker, Browser, Terminal, Snip, Paint, Images, Media, Task Manager and Settings. Their detailed capabilities and limits remain in [First-party apps](FIRST_PARTY_APPS.md) and the archived inventory.

Removed controls are deliberate product changes, not outstanding work. Unchanged capability limits include optional CPU temperature sensor providers, unavailable temperature/usage metrics on unsupported hardware, unmeasured startup impact, the text editor's large-file editing limit, and the image editor's existing color/format limits. These do not create new backlog commitments.

## Verification evidence

| Validation | Result and artifact |
|---|---|
| Release build | Successful, zero warnings and errors. |
| Complete first-party app run | **206/206 passed** in cleanup-apps-verified/report.json (`artifacts/cleanup-apps-verified/report.json`, local evidence). Covers app surfaces, earlier Discard fixes, picker behavior, Task Manager transparency/outline, Settings, migration, bindings, animations and tray callbacks. |
| Final widget/desktop run | **485/485 passed** in cleanup-widget-visuals-final/report.json (`artifacts/cleanup-widget-visuals-final/report.json`, local evidence). Includes eight widgets across all 16 slots, YouTube frame sizing, native input probes, crowded columns, drag scrolling, monitor/workspace sequences, fullscreen and bar boundaries. |
| Final tray and Settings follow-up | **68/68 passed** in cleanup-tray-settings-final/report.json (`artifacts/cleanup-tray-settings-final/report.json`, local evidence). Exercises exact callback order for protocol versions 0, 3 and 4, including direct right-click, plus final Settings refinements. |
| Tailscale registration follow-up | **31/31 passed** in tailscale-tray-20260905/report.json (`artifacts/tailscale-tray-20260905/report.json`, local evidence). Adds real `ABM_GETTASKBARPOS` queries without Explorer for top/bottom bars at negative coordinates and verifies that modern Open leaves a toggle menu open. Corrects the earlier double-selection expectation. |
| Quiet self-tests | **144/144 passed** in cleanup-final-self-test.json (`artifacts/cleanup-final-self-test.json`, local evidence). Includes the current-schema migration check and native outline ownership. |
| Change hygiene | Final diff whitespace check passes. Existing local follow-up fixes were preserved. |
| Visual review | Settings previews at 640×600 and 1040×600; expanded previews for all eight widgets. Final screenshots accompany the app and desktop reports. Native browser sizing and input checks supplement raster previews. |

The suites overlap; their totals should not be added together as unique tests. They run in isolation and do not change the installed shell or VPN connection. Tray packing follows the [Windows notification-icon contract](https://learn.microsoft.com/en-us/windows/win32/api/shellapi/nf-shellapi-shell_notifyicona), retaining the installed [ManagedShell version's compatibility behavior](https://raw.githubusercontent.com/cairoshell/ManagedShell/87d0736386a94acb077bee05f4093ce1f7fd6d9f/src/ManagedShell.WindowsTray/NotifyIcon.cs).

## What remains before calling this live-accepted

1. Release `20260905-110411` is now running after the user's restart. Live Sound Settings output switching, volume and mute/unmute were exercised and the original visible configuration restored. Subsequent dropdown/focus fixes are built and tested in source but not yet installed; see the latest acceptance report.
2. Physically exercise Antigravity/Tailscale Open and native-menu behavior. New tests cover window restoration, popup preservation and guarded activation fallback; real VPN connection changes remain untested.
3. Check Task Manager transparency and overlap, Text/Snip Discard, the save picker, Floating maximize and the reported workspace sequence in the installed shell.
4. Use the widgets in preferred slots with real browser content and judge the animations on the actual displays. The fixes improve pacing, cancellation and layout; no software test can guarantee perfectly smooth animation on every computer under every workload.

The complete current acceptance list, including sound, native controls, widget startup and the desktop-control limitation, is in [Follow-up acceptance](FOLLOWUP_ACCEPTANCE.md). New defects found during use can start the fresh backlog. Phase 5 is approved; outstanding physical acceptance remains tracked here.
