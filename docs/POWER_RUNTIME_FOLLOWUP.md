# Power, runtime and stability follow-up

September 5, 2026. Work is in `C:\Projects\StrataShell`, on the local Preview branch. This report separates implemented source, isolated package tests and installed acceptance.

## Completed in this pass

| Request | Implemented behavior |
|---|---|
| .NET 10 | Shell, Launcher, Maintenance and Setup target .NET 10. SDK selection, CI, development scripts, release validation and Launcher checks agree on the new version. The candidate was built and run with SDK 10.0.400 / runtime 10.0.11. |
| Stable-only runtime packaging | Stable includes the .NET Desktop runtime and Windows Forms dependencies. Preview remains framework-dependent with no bundled runtime, full Setup or wallpaper library. A .NET 10 Preview needs a compatible installed .NET 10 Desktop runtime or an exact matching runtime bridge from a compatible Stable installation; the older .NET 9 Stable runtime cannot supply it. |
| Battery tray menu | The battery/power icon opens battery percentage, charging/on-battery state, available capacity/health and rate, Energy Saver status, and native Windows power controls. Shutdown/restart/session controls remain separately accessible. |
| Windows power settings | Select Best power efficiency, Balanced or Best performance independently for AC and battery where Windows exposes them, or choose an installed power plan. Writes reject stale selections and verify Windows retained the result. Unsupported/policy-controlled settings report their actual limitation. |
| Automatic Low quality | Automatic power saving temporarily disables expensive blur/bloom, uses economical wallpaper scaling and caps rendering at 60 Hz. It applies even when the saved preference is High. It activates for Windows Energy Saver, Best power efficiency, or battery charge at/below 20% while unplugged. The low-charge condition clears at 25% or on AC; any still-active saver condition continues to apply. Saved quality and individual preferences are never rewritten and resume when all triggers clear. The setting can be disabled. |
| Battery usage | An on-demand Windows battery report supplies up to 14 days of total active time on battery and AC. This is historical usage time, not per-application energy attribution. Generation is bounded, asynchronous and cleans its temporary report. |
| Windows Update | Native availability/history stays in STRATA. Open Windows Update handles installation and scheduling in Windows Settings. Optional Start/Stop temporary Explorer supports that page when needed, leaves STRATA running and never changes the selected next-login shell. Stop targets only the tracked session, not an Explorer process that was already running. |
| YouTube expanded lock | Reapplying a saved expanded widget now retries the player/surface restoration after an interrupted startup/layout operation. A retained/cancelled player no longer waits for the first hover to finish expanding. |
| Settings sliders | Rebuilding the current settings page restores its scroll position after layout. Navigation to another page still starts at the top. |
| System information | Cached Windows hardware queries replace placeholders for motherboard, BIOS, CPU counts/cache and RAM speed/form factor. A running hypervisor is distinguished from the processor firmware flag. Performance and System Info use the same cached inventory. All four System Info cards keep aligned outer edges and padding in compact and wide layouts. |
| Complete uninstall | Owned read-only caches are removable; folder links are removed without deleting their external targets. Remove-settings uninstall deletes program files, caches, backgrounds and settings. Keep-settings preserves the exact profile. Registration is removed only after cleanup succeeds, so a cleanup failure remains retryable. |
| Tray recovery exception | The earlier local tray fix remains included: readers use a stable registration snapshot rather than enumerating a collection that native callbacks can change during a refresh. |
| Wallpaper hang mitigation | Wallpaper changes persist their selection quietly, avoiding an unrelated full settings/layout broadcast before the prepared wallpaper/frost transaction. Stall diagnostics now name the current theme subscriber or wallpaper presentation operation. |

## What remains limited, and why

- **Windows per-monitor DPI:** display resolution, refresh, placement and orientation have native controls. This pass does not use undocumented Windows display packets to change DPI. Display Settings now provides a direct Windows Display Scale route; STRATA's own scale remains separate.
- **Immediate Windows Energy Saver:** the current Windows status API is read-only. Windows power modes are implemented above, but Best power efficiency and Energy Saver are different Windows features. STRATA exposes the existing automatic threshold and a direct Energy Saver Settings route for the immediate Windows toggle.
- **Windows Update installation/scheduling:** Windows has update APIs, but a complete privileged installation, policy, license-consent and restart workflow is not implemented here. The requested Windows Settings/temporary Explorer fallback is provided.
- **CPU temperature:** motherboard and BIOS data are ordinary system inventory; CPU temperature is a sensor reading. STRATA reads an already-running Libre Hardware Monitor/Open Hardware Monitor provider. It does not install a privileged sensor driver. NVIDIA temperature continues through the installed driver's NVML interface. Unsupported readings remain unavailable, including CPU temperature when no provider exists.
- **The reported hang:** Windows recorded an AppHangB1, not a managed exception with a stack. It followed the dark-wallpaper shortcut, a theme update and an unresponsive dispatcher; the supervisor restarted the previous installed release. The local mitigation addresses the observed update path, but the available logs do not establish its exact blocking call. Repeat installed testing is needed before calling this resolved.

## Verification

All four Release components built without warnings or errors. The official .NET SDK download was checked against Microsoft's published SHA-512 before use. Local evidence is under `artifacts/pre-phase5-followup/`:

| Evidence | Result |
|---|---|
| `all-apps/report.json` | 339 application checks passed. |
| `desktop/report.json` | 556 desktop/widget checks passed, including cancelled YouTube startup restoration. |
| `self-test.json` | 144 quiet self-tests passed on .NET 10. |
| `defaults/report.json` | 22 fresh-install and existing-profile preservation checks passed. |
| `maintenance/report.json` | 11 maintenance/runtime/deletion checks passed. |
| `runtime-lifecycle/report.json` | 9 isolated Stable/Preview payload and install/reinstall/uninstall checks passed. |
| `final-cards-native/report.json` | 83 focused hardware, native-settings and card-resize checks passed; compact cards and lower hardware rows were visually reviewed. |

The lifecycle harness publishes explicit temporary test fixtures. It runs Setup and the installed Launcher without depending on a globally registered runtime, preserves settings across reinstall, removes read-only caches, preserves a file outside a directory junction, and removes shortcuts/registration. It does not produce or promote a distributable Stable package or bypass the clean-main release gate.

The owner reports the existing Bluetooth, microphone/audio, Wi-Fi, display and input/power controls work as intended. That is owner-reported acceptance of existing controls; the new battery-mode transitions, temporary Explorer workflow, exact startup sequence and hang mitigation still need the updated candidate installed for physical acceptance. Automated checks and still images cannot guarantee animation smoothness on every computer.

## Deployment status

The last verified running installation is Preview `20260905-135348`, source `e3588b9`, on .NET 9.0.19. The local .NET 10 changes in this report have not been committed, pushed, published, installed or used to restart the active shell in this pass. Existing installed packages retain their own runtime and behavior.

README/current documentation have been updated locally in both the source and distribution checkouts. Historical release notes remain historical. Publisher signing and the retired laptop investigations remain outside the active scope.

## References

- [.NET support policy](https://dotnet.microsoft.com/en-us/platform/support/policy) and [official .NET 10 release metadata](https://builds.dotnet.microsoft.com/dotnet/release-metadata/10.0/releases.json).
- [Windows AC power-mode selection](https://learn.microsoft.com/en-us/windows/win32/api/powrprof/nf-powrprof-powersetuserconfiguredacpowermode) and [battery power-mode selection](https://learn.microsoft.com/en-us/windows/win32/api/powrprof/nf-powrprof-powersetuserconfigureddcpowermode).
- [Windows powercfg and battery-report options](https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/powercfg-command-line-options).
- [Windows Update installer interface](https://learn.microsoft.com/en-us/windows/win32/wua_sdk/iupdateinstaller-properties).
