# Native settings, bar and wallpaper follow-up

September 5, 2026 · `C:\Projects\StrataShell` · local Preview source.

This records the original cleanup/native-wireless implementation. Phase 5 is now active; see [current status](STATUS.md). Current follow-up fixes, deployment status and testing limits are in [Follow-up acceptance](FOLLOWUP_ACCEPTANCE.md). Hardware reads and isolated test fixtures do not establish physical acceptance of device mutations.

## Implemented

| Area | Current behavior |
|---|---|
| Display | Draggable monitor map and exact positions; resolution, refresh, orientation, primary/enabled displays; Extend/Duplicate. A separate current-user pipe helper validates and previews changes, restores after 20 seconds/disconnection, and saves only after Keep. Stale/hotplugged drafts are rejected. |
| Input | Native mouse speed, acceleration, primary button, wheel and double-click controls; keyboard repeat; supported Precision Touchpad parameters; STRATA swipe enable/threshold. Separate keybind viewer/editor links replace the inline shortcut list. |
| Startup | Search/browse the launcher catalog and select apps to start with the installed shell. Stable IDs are resolved against the current catalog. Preview/tests do not launch them; missing selections remain removable. Selections run again on shell restart. |
| Power | Active-plan AC/battery screen and sleep timeouts; available battery charge/capacity/rate and Energy Saver threshold. The current follow-up adds separate AC/battery modes, installed plan selection, battery tray controls, automatic temporary Low effects and 14-day usage history. Policy/driver limitations are reported. |
| Sound | Shared Settings/tray output and microphone selection, levels and mute; per-session application mixer; bounded polling only while visible. |
| Date + Time | Timezone and DST choice, current Windows time-source synchronization, validated manual local time through a narrow elevated helper. Tray clock opens a month-navigation calendar. |
| Quick Controls | Updated native sound, display, input, startup, time and power routes plus matching launcher commands. Compact status strip keeps the clock and essential hardware controls. |
| Windows Update | On-demand native availability/history; Open Windows Update for installation and scheduling, plus explicit Start/Stop temporary Explorer when that Windows page needs it. |
| Explorer/default shell | Routine native controls do not require Explorer. The current follow-up restores an explicit, tracked temporary Explorer bridge for Windows Update, preserving preexisting Explorer sessions and the chosen next-login shell. Display scale and Energy Saver have direct Windows Settings routes. Automatic missing-payload/launch/crash-loop recovery now starts Explorer for the session without changing the selected next-login shell. The explicit Windows Tweaks default-shell switch and emergency restore retain their intended behavior. |
| Document windows | Browser and Media launches now join Files, Text, Paint, Images and Terminal in opening independent document windows. Browser windows share profile storage to avoid stale bookmark/history/settings overwrites. Utility singletons remain utility singletons. |
| Floating bar | Group counts, hover/group-click previews, Shift-click/new-window action, individual Close and Close all. Close sends normal window requests and preserves cancel/unsaved-document handling. Preview pages limit compositor surfaces and release them on dismissal. Minimized windows show restore cards. |
| Minimize | Windows shell-hook requests receive the app icon's physical rectangle, including scrolled dock edges. STRATA apps use a short DWM-thumbnail transition with no per-frame screenshot capture or window-layout movement; real restore bounds are retained. Reduced motion/low effects bypass it; fullscreen dismisses shell previews/transitions. |
| Browser toolbar | Only Menu beside the tabs; Bookmarks/History moved onto the main toolbar; one Downloads button retains its activity badge; developer-tools toolbar button removed. |
| Wallpaper | Strong frost uses a half-resolution filtering pass, restored to the same texture dimensions. Wallpaper resolution, bloom and shadows remain configured as before. Foreground requests attach before speculative warming is canceled, allowing in-progress neighbor work to be reused. Image, palette and every required frost texture prepare before publication; wallpaper and frost crossfade together using the same duration/easing. |
| Widget handoff | Interrupted height animations cannot let an old completion overwrite a newer hover/collapse; the intended final height is stored before animation starts. |

## Capability boundaries

- Windows per-monitor DPI is read-only here; STRATA UI scale is separate. HDR/Night Light, language/IME and vendor-specific controls are outside this implementation.
- Precision Touchpad controls require a supporting device/Windows version. The native V1 device-enabled field is read-only; unsupported options are not presented as working controls.
- Historical total active time on AC/battery is available for up to 14 days. Per-app attribution and immediate native Energy Saver switching are not implemented; the latter has an explicit Windows Settings route. Battery fields and timeouts depend on hardware/active plan/policy.
- Windows Update installation, pause and restart scheduling remain Windows-owned in this pass. No update installation is initiated by a check.
- UAC, secure sign-in, driver consent and protected authentication remain Windows trust surfaces. Time changes can require Windows elevation.
- DWM can decline/omit previews for protected or unavailable surfaces. External applications/Windows animation preferences control whether a minimize animation is rendered; STRATA supplies the requested icon target. Source tests cannot guarantee smoothness on every machine.

## Verification

- Release build: zero warnings/errors.
- Combined display, native Settings, sound/calendar, browser and wallpaper checks: **112/112** in `artifacts/native-settings-final/report.json`.
- Quiet self-tests: **144/144** in `artifacts/native-settings-self-test.json`.
- Desktop/widget/dock regression: **493/493** in `artifacts/native-settings-dock-verified/report.json`. The first run found an interrupted widget-animation completion race; that was corrected before the passing rerun.
- Read-only native display, mouse, audio, power and wireless inventories were exercised. Display confirmation, timeout, failure, disconnection and hotplug restoration use injectable controllers; tests did not change this computer's display, radio, input, audio, power or clock settings.
- Captured and reviewed browser chrome, native Settings, light/dark sound, display and preview layout. WPF captures omit DWM thumbnail content; installed compositor acceptance remains separate.

The isolated frost benchmark used the same generated 3200×1800 image and 50% blur. Cold landscape preparation improved from **279 ms to 147 ms**, and portrait from **242 ms to 122 ms**. Cached requests stayed below 1 ms. The integrated run measured 170/115 ms under concurrent test load. These are preparation timings on this development machine, not full switch latency or a promise for all hardware. Sampled RGB mean difference was **1.34/255** against the previous landscape frost; the main wallpaper image is unchanged. Before/after artifacts are `artifacts/wallpaper-before` and `artifacts/wallpaper-after`.

## Installed acceptance still required

1. Apply/Keep/Restore real display changes, including mixed DPI, rotation, multiple adapters, hotplug and shell exit during preview.
2. Exercise available touchpad gestures, native input controls, AC/battery timeouts, audio device changes/mic mute/app mixer, timezone/sync and selected startup apps.
3. Check Bluetooth pairing with real mice/keyboards/headsets/speakers and Wi-Fi radio/disconnect/forget. See [native wireless acceptance](NATIVE_WIRELESS_ACCEPTANCE.md).
4. Open several Browser/Files/Text/Paint windows; choose previews, cancel a dirty-document Close, minimize/restore on each monitor and enter true fullscreen games. Inspect actual compositor thumbnails and frame smoothness.
5. Switch High Quality wallpapers repeatedly and confirm wallpaper/frost change together, including rapid switches and different monitor shapes. Verify the simplified toolbar and clock/calendar with actual pointer input.
6. At the next approved deployment, verify the selected default-shell policy remains unchanged after a temporary Explorer session. No live policy repair was performed in this pass.

Native references: [Windows shell minimize notifications](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-registershellhookwindow), [DWM thumbnail ownership/lifecycle](https://learn.microsoft.com/en-us/windows/win32/api/dwmapi/nf-dwmapi-dwmregisterthumbnail).
