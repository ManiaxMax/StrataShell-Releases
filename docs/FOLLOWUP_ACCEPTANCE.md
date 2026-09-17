# Tray, sound and startup-widget follow-up

September 5, 2026. Source: `C:\Projects\StrataShell`, Preview branch. Phase 5 is now active; see [current status](STATUS.md).

**Installed and running:** `20260905-110411`, with 144/144 installation self-tests passing. After the user restarted STRATA, the bootstrap and shell processes and the open Settings window were verified against this release path. The existing STRATA default-shell selection and saved clock/YouTube expansion locks were preserved. The additional sound/dropdown polish below is built and tested in source, but is not yet packaged or installed. No commit, push or release publication was performed.

The complete candidate is `artifacts/followup-candidate/StrataShell-20260905-followup-complete`. The earlier `followup-validation` and `followup-fixed` staging folders are rejected/incomplete builds, retained only for failure evidence.

## Changes

- **Speaker button:** opens the native sound popup instead of muting. Output and microphone device selection, levels, explicit Mute/Unmute buttons, and the full mixer route remain available. Percentage readings are separate from mute actions.
- **Saved widget locks:** both columns restore expanded geometry and interactive details during initial layout. A reproduced retained-player refresh could compact the locked YouTube card to 150 pixels; the corrected result remains 385 pixels. Opacity transitions settle to their newest target, so an interrupted collapse cannot hide newly expanded controls.
- **Tray Open:** restores the publisher's most recently used tracked window, including minimized/off-workspace windows. Otherwise it opens the native app menu. An app such as Antigravity supplies its window-creation action there; tray-only applications such as Tailscale expose their controls there. For click-only publishers, a bounded fallback sends their activation gesture if no interactive surface appeared. New input or another panel action cancels the fallback. There are no app-name exceptions, process restarts or forced exits.
- **Tray focus:** grants foreground permission while STRATA's panel still owns focus, then hides the panel before dispatching native callbacks. Existing Tailscale taskbar-presence support is preserved.
- **Package dependencies:** self-contained WPF-only publishing omitted Windows Forms/Drawing assemblies used by ManagedShell. Packaged tray testing exposed the failure; Shell and Launcher now include a matching complete desktop runtime. Payload/archive checks and the quiet notification-area self-test detect the missing dependencies. The installed older framework-dependent Preview passed its 31 tray checks, so this packaging defect is not established as the cause of its Open behavior.

Windows notification icons do not expose a universal command to create an application window. For a closed Antigravity window, the supported path is Open → its native menu → Open Antigravity. This is distinct from restoring an existing window directly. Antigravity's installed app code registers its reopen handler in that menu, and its second-instance handler does not create a missing window.

### Findings after the live restart

- The running Sound + Mixer page was operated with keyboard input. Speaker volume changed from 15% to 16% and back; Mute/Unmute responded; switching from Speakers to the ARZOPA monitor output updated the displayed default device and its level. Speakers was restored at 15%, unmuted. Application audio sessions were listed. No microphone was detected, and audible playback quality and microphone operation were not established.
- Opening the real device dropdown exposed internal `SoundEndpoint` record text. The shared dropdown-item template omitted `ContentTemplateSelector`, bypassing friendly names supplied by `DisplayMemberPath`. The source template now forwards it; the isolated rendered dropdown was inspected and shows only device names.
- Changing the default device rebuilt the panel and lost keyboard focus. Source now preserves the device selector's focus during a required rebuild, updates unchanged controls in place, and retains a requested device refresh arriving during an in-flight poll. A late response does not reclaim focus after the user moves elsewhere.
- Live keyboard navigation also stopped on the invisible increment/decrement regions inside sliders. Those regions now leave keyboard focus on the slider itself, with the next Tab advancing to the next real control.

These additional corrections passed a zero-warning Release build, **41/41** focused sound/native Settings checks (`artifacts/followup-live-sound-regression/report.json`, local evidence), and **71/71** related display/Settings checks (`artifacts/followup-shared-controls-regression/report.json`, local evidence). The sound checks cover friendly dropdown labels, focus retention, concurrent polling, focus not being stolen, and Tab navigation in both full Settings and the compact tray panel. These corrections are source-only until the next deployment.

## Verification

| Check | Result |
|---|---|
| Release build | Passed, zero warnings/errors |
| Installed candidate: desktop, widgets, dock, workspace, fullscreen and speaker routing | 497/497 on an isolated desktop, report (`artifacts/followup-installed-desktop/report.json`, local evidence) |
| Full first-party apps and native settings with the corrected runtime | 312/312, report (`artifacts/followup-candidate-fixed-apps/report.json`, local evidence); tested shell assembly hash matches the final package |
| Final tray behavior | 41/41, including popup preservation, one fallback, cancellation and minimized-window restoration, report (`artifacts/followup-tray-verified/report.json`, local evidence) |
| Final native settings/sound | 29/29, report (`artifacts/followup-sound-final/report.json`, local evidence) |
| Packaged self-tests | 144/144, report (`artifacts/followup-candidate-self-test.json`, local evidence) |
| Installed candidate self-tests | 144/144, report (`artifacts/followup-installed-self-test.json`, local evidence) |
| Package contract | Final folder and ZIP passed, including all three previously omitted tray runtime assemblies; the earlier incomplete package is rejected by the new check |

The suites overlap; these counts are not additive. Isolated captures were inspected for the sound controls and browser. Tests run on a private Windows desktop with isolated settings and without changing Windows theme preferences. They do not establish physical audio, wireless, display or animation acceptance.

## Live acceptance and remaining work

The user authorized hands-on testing and local fixes. Live capture still failed after restarting and selecting Settings: `IGraphicsCaptureItemInterop.CreateForMonitor` returned `0x80070057` on both attempts. Indexed mouse input then reported `coordinate input geometry is unavailable`; the range-value setter also failed inside the control tool. Keyboard input and accessibility reads worked and were used for the live sound checks above. The desktop-control tool exposes application windows but currently does not expose STRATA's bar or desktop. Antigravity could previously be launched and read through accessibility, but live tray clicks and animation observation remain unverified. No blanket claim of smooth animations or complete physical acceptance is made.

1. The restart is verified. Physically verify the speaker-button popup route and tray Open for Antigravity, Tailscale and other installed apps, with both existing and closed windows. Deploy and verify the subsequent source-only dropdown/focus fixes when ready.
2. Observe a real shell startup: both saved expanded widgets must appear expanded before hovering, and retain working controls through layout/theme changes.
3. Physically confirm Text/Snip Discard, picker outlines, Task Manager transparency/overlap, Floating maximize/bar boundaries and multi-monitor workspace switching. Their isolated regressions pass.
4. Output switching, master volume and mute/unmute were exercised in live Settings. Microphone operation, per-app mixer mutations and audible results remain pending, along with supported display changes/rollback, input controls, startup selections, power/time settings and wireless actions. Preserve the user's original settings. Real Bluetooth pairing requires an available peripheral in pairing mode.
5. Observe compositor previews, native/STRATA minimize and workspace animations, true fullscreen, and synchronized High Quality wallpaper/frost transitions on the actual monitors. Static captures and frame-policy tests cannot establish perceived smoothness.
6. Verify temporary Explorer use retains the selected default shell during a live session transition.

The detailed native-control limits remain in [Native settings acceptance](NATIVE_SETTINGS_ACCEPTANCE.md). Windows per-monitor DPI writes, vendor-only controls, historical battery usage/immediate Energy Saver and Windows Update installation/scheduling are outside the implemented controls. Laptop-specific investigations and publisher signing remain deferred, as requested.

Native behavior references: [foreground permission](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-allowsetforegroundwindow), [Electron tray click/menu handling](https://github.com/electron/electron/blob/main/shell/browser/ui/win/notify_icon.cc), [Walk notification-menu handling used by Tailscale](https://github.com/tailscale/walk/blob/master/notifyicon.go).
