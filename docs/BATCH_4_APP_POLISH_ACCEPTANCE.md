# Phase 4 app polish and workspace routing acceptance

Date: 2026-09-05. Authoritative source: `C:\Projects\StrataShell`, branch `polish/phase-4-apps`, based on `3a9b563`. Status: local source candidate; installed acceptance remains open under STRATA-040.

The running release observed during this work was `20260904-234627`. These corrections are in the new local build, not that installed release. No Phase 4 source commit, push, publication, installation or live-shell restart occurred.

## Workspace switching defect

The reported sequence is Super+Ctrl+Up to the primary monitor, Super+Ctrl+Down back to the secondary, then repeated Super+Ctrl+Left/Right or Super+Tab in tiling mode. A previous monitor's delayed focus retry could survive a move to an empty destination. In addition, the native foreground-event path bypassed the existing manual monitor-selection guard. Automatic Windows focus fallback could therefore redirect later workspace commands to the primary monitor.

Explicit monitor/workspace selection now retires the earlier pending focus request. Foreground notifications honor the selected-monitor guard and presentation transaction. Deliberate pointer selection and Alt+Tab release the guard so normal app selection can change monitors. The workspace shortcuts share the corrected routing.

This was verified by a comparison, not only a passing test: a private copy of the current candidate with the old focus behavior restored failed 53 of 200 checks. Its first failures were the empty-secondary Tiled Up/Down and Right/Tab sequences. The corrected source passed all 200 checks, including empty/occupied secondary workspaces and repeated switches in Tiled and Floating modes. The comparison copy is not an exact reconstruction of an earlier release and must never be installed.

Evidence:

- Corrected routing: `artifacts/phase-4-desktop-accepted/report.json` — 200 passed, zero failed.
- Old-behavior comparison: `artifacts/phase-4-workspace-before/report.json` — 53 failed; comparison only.
- The expanded runner's earlier 45-second time limit was increased to 90 seconds. The corrected full run completed successfully.

These private-desktop tests invoke the same command methods and focus paths, including delayed previous focus. Physical shortcuts in the newly installed build remain the final acceptance step. A passing isolated run does not invalidate the user's observation in the older installed shell.

## App changes

| Area | Implemented behavior | Main verification / boundary |
| --- | --- | --- |
| Shared Open/Save dialogs | Suppress the covered owner's active highlight, including delayed callbacks; restore after dismissal. | Actual shared Open and Save pickers exercised for Text, Paint, Images and Snip. |
| Text | Worker-based cancellable reads/saves, 4 MiB limits, snapshot/generation guards, external-change detection, strict legacy encoding, encoding/line-ending choices, unsaved-close protection. | External edits are preserved; BOM, LF/CRLF and download provenance tested; stale open completion rejected. |
| Files | Copy/paste/drop progress and cancellation, bounded directory preflight, partial-success reporting, cross-volume safety, native copy preserving alternate streams, archive cancellation, compact toolbar overflow. | Mid-file cancellation removes incomplete output; successful copy preserves bytes/streams; mixed good/missing inputs report partial success. Recycle operation remains synchronous. |
| Terminal | Real ConPTY interactive input/output with embedded xterm.js 6.0.0 and Fit 0.11.0; bounded queues, acknowledgements and lifetimes; resize, Unicode, Ctrl+C, selection copy/paste/find/export; fallback mode. | Real ReadKey/Unicode/resize and browser-host input/output/Ctrl+C checks. Network/navigation and external assets are denied. No arbitrary OSC clipboard bridge. Long-running third-party terminal applications still need installed use. |
| Browser | Per-origin permission prompts and remembered-choice review/reset; reject stale/background requests; live download progress/cancellation; completed-only Open; theme-correct new tab. | Permission persistence/reset, origin handling and native Light/Dark captures. Physical camera/microphone consent and website compatibility remain installed checks. |
| Snip | Bounded capture budget, worker processing, frozen save snapshots, revisions/unsaved prompts, cancellation and compact toolbar. | Capture-budget and real snapshot/picker checks. An 8K-sized capture fits the pixel budget; actual high-resolution capture and annotation input need hardware acceptance. |
| Images | All eight EXIF orientations, exact pixel rotate/flip export, bounded sibling discovery, asynchronous/stale-result guards and overflow controls. | Exact pixel order verified for orientations/transforms. SDR pipeline; HDR fidelity is not claimed. |
| Paint | Recheck document/drawing/lifetime state after awaited dialogs; retain compact toolbar access. | Shared picker and theme/layout checks. Existing bounded history/fill/import protections retained. |
| Media | Real shared system-output spectrum replaces random animation; subscribe only when visible, playing and motion-enabled; bounded asynchronous playlists and playback-state/error cleanup. | Both themes/compact layouts checked. Audible playback, codecs and seeking were not physically accepted in this phase. Spectrum represents system output, not isolated player audio. |
| Task Manager | Native CPU clock/disk/GPU counters, adapter selection and separate histories, supported temperatures, correct hardware-page labels, compact navigation, network filter exclusion, process identity checks. | Actual host counters and GPU temperature recorded; all eight app sections and five performance pages rendered. No real process termination or priority changes performed. |
| Settings/shared surfaces | Live theme resources, accessible names from tooltip labels, compact responsive groups, text/control contrast and toolbar access. | Ten app surfaces rendered in both themes at three raster scales. Raster scaling does not prove native DPI transitions. |

The embedded terminal assets include license/provenance metadata and were checked against official package SHA-512 digests. App test profiles and fixtures are isolated from the user's normal profiles.

## Hardware telemetry: what works and what remains unavailable

Windows PDH supplies CPU frequency, logical-volume idle/read/write/latency, GPU engine utilization and dedicated/shared memory. Registration uses English counter names through the language-neutral API. One persistent query runs on the existing sampling worker; provider discovery backs off, buffers are bounded and disposal does not block WPF. No new shell-wide telemetry timer was added.

GPU instances are matched by DXGI adapter LUID. Processes sharing an engine are aggregated, then the busiest engine represents load; parallel engines are not added into a fictional overall percentage. This host exposes three RTX 4070 DXGI identities, with counters available on the first. The selector retains distinct identities rather than merging them by name. Missing counter instances remain unavailable.

NVIDIA temperature comes from the installed driver's System32 NVML library. The tested RTX 4070 reported 46 °C in the focused temperature capture; a subsequent independent `nvidia-smi` reading was 50 °C. These are different sample times, not a calibration comparison. Source labels identify the driver reading. Ambiguous identical physical card models are not assigned temperatures by guesswork.

CPU temperature support reads an already-running Libre Hardware Monitor or Open Hardware Monitor WMI provider. There was no such provider here: CPU temperature remained null with an explanatory message. CPU provider detection, hardware accuracy and sensor naming on other PCs remain acceptance work. STRATA did not install a driver/service, request elevation or launch a hardware-monitoring application. ACPI thermal zones are not substituted for CPU package temperature.

Network totals now exclude addressless WFP/QoS filter interfaces that repeat Ethernet byte counts; the observed host previously counted the same traffic four times. Totals cover IP-configured adapters and can still include traffic at both a VPN interface and its underlying transport. They are not unique physical wire throughput. The graph displays MB/s, and the connection detail identifies the adapter it describes.

Remaining unsupported measurements are explicit: AMD/Intel GPU temperatures, power draw, per-core usage, RAM speed/form factor, memory pools/cache, detailed motherboard fields, per-process disk/network/GPU and user-session accounting. Startup impact is not measured. Missing, denied and warming-up readings are not silently displayed as zero.

## Final local validation

| Check | Result | Evidence |
| --- | --- | --- |
| Release build | Passed; zero warnings/errors | `dotnet build ... -c Release --no-restore` |
| Quiet safety/regression suite | 144 passed, zero failed | `artifacts/phase-4-checks/self-test.json` |
| Full app acceptance | 117 passed, zero failed | `artifacts/phase-4-final-apps/report.json` |
| Desktop/workspace regression | 200 passed, zero failed | `artifacts/phase-4-desktop-accepted/report.json` |
| Focused temperature acceptance | 27 passed, zero failed, before final network/sidebar refinements | `artifacts/phase-4-temperatures-accepted/report.json` |
| Final Task Manager display refinement | 28 passed, zero failed after preserving fractional MB/s axis labels | `artifacts/phase-4-final-task-labels/report.json` |
| Old focus behavior comparison | Fails as expected: 53 failed of 200 | `artifacts/phase-4-workspace-before/report.json` |

Full app acceptance covers Text, Files, Browser, Terminal, Tasks, Media, Snip, Paint, Images and Settings. Light/Dark renders use 1000×650 DIPs at 100%, 854×480 at 150%, and 640×480 at 200%. Browser/terminal native content captures and contrast checks supplement WPF renders. Screenshots and `task-hardware.json` are beside the reports. Hardware JSON is local diagnostic evidence and contains machine/network details; review/redact before external sharing.

The final change after the full app suite was graph-label decimal formatting: small network scales now show distinct 0.25/0.5/0.75 MB/s ticks. A clean Release rebuild and the focused Task Manager suite above verify that refinement. No shell-routing changes followed the 200-check desktop pass.

## Installed and hardware acceptance still required

1. Run the corrected candidate, verify its executable path/version, and repeat the user's physical monitor/workspace shortcut sequence in tiling mode with empty and occupied workspaces. Confirm the primary workspace remains unchanged while operating on the secondary.
2. Open/Save from each editing app and confirm no owner highlight overlaps the picker; cancel and complete dialogs, then verify focus/highlight restoration.
3. On the Zenbook MX450 and Iris Xe laptops, move Browser and native app surfaces between displays, including the half-height display enabled/disabled, mixed DPI, 60 Hz and rapid workspace changes. Record freezes, input delay and resource use. The RTX 4070 host cannot prove this issue resolved.
4. Exercise file cancellation, disconnected shares, save conflicts, terminal interactive programs, permission prompts and real media playback in normal use. Test sleep/resume and display docking with these apps open.
5. Verify GPU telemetry on NVIDIA/Intel/AMD target drivers and CPU temperature with a supported provider. Confirm unsupported readings remain clear. Do not treat this phase as universal sensor coverage.

The backlog remains in progress until installed acceptance. The report distinguishes completed implementation and local evidence from unverified hardware behavior.

## Primary implementation references

- [Microsoft: creating a pseudoconsole session](https://learn.microsoft.com/en-us/windows/console/creating-a-pseudoconsole-session) and [xterm.js security guide](https://xtermjs.org/docs/guides/security/).
- [Microsoft: CopyFileExW](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-copyfileexw).
- [Microsoft: English counter registration](https://learn.microsoft.com/en-us/windows/win32/api/pdh/nf-pdh-pdhaddenglishcounterw), [formatted counter arrays](https://learn.microsoft.com/en-us/windows/win32/api/pdh/nf-pdh-pdhgetformattedcounterarrayw), and [GPU utilization semantics](https://devblogs.microsoft.com/directx/gpus-in-the-task-manager/).
- [NVIDIA: NVML device queries](https://docs.nvidia.com/deploy/archive/R535/nvml-api/group__nvmlDeviceQueries.html) and [Libre Hardware Monitor WMI example](https://github.com/LibreHardwareMonitor/LibreHardwareMonitor/blob/master/LibreHardwareMonitor.Windows.Forms/TestScripts/basicwmi.py).

## September 5 follow-up: Discard close and external maximize

The earlier delivery paragraph records the original Phase 4 pass. This follow-up starts from committed Phase 4 source `b62e049`, with installed/running release `20260905-011638` observed on the machine. Current follow-up branch: `fix/discard-close-reentrancy`. The follow-up changes have not been committed, published or installed.

**Text and Snip:** installed logs identify `InvalidOperationException` from their closing handlers after Discard: WPF prohibits a second `Close()` while the original closing event is executing. An `await` does not yield when the confirmation completes synchronously. Both handlers now queue the approved close after the canceled event returns, matching Paint's existing behavior. Private tests exercise real unsaved windows, actual confirmation buttons, canceled Save pickers, successful saved files, retained dirty state on cancel and exactly one closed event.

**Floating maximize:** read-only live geometry showed both STRATA docks visible while each monitor's native work area still equaled its full monitor rectangle. ChatGPT has ordinary caption/maximize styles; its normal maximize therefore lacked the WPF-specific bounds guard. Native reconciliation now applies STRATA's reservation to normal maximized external apps, compensates DWM frame margins and preserves restore geometry. The test fixture uses a raw native HWND without WPF sizing hooks and deliberately starts at full-monitor bounds. Top and bottom edges are checked on both monitors. True-fullscreen tests remain part of the full desktop run.

| Follow-up validation | Result | Local evidence |
| --- | --- | --- |
| Release build | Passed, zero warnings/errors | Current source build |
| Text/Snip before fix | Both close-reentrancy exceptions reproduced; two corresponding Discard checks failed | `artifacts/discard-close-reproduced/report.json` |
| Text/Snip after fix | 37 passed | `artifacts/phase4-followup-apps-final/report.json` |
| External maximize before fix | Four top/bottom boundary failures; 208 total checks | `artifacts/external-maximize-before/report.json` |
| External maximize after fix | 208 passed; native visible frames match expected work areas exactly on this host | `artifacts/external-maximize-fixed/report.json` |
| Quiet regression | 144 passed | `artifacts/external-maximize-fixed/self-test.json` |

Installed physical ChatGPT maximize/double-click and Text/Snip Discard still need acceptance after installing these follow-ups. The user's running shell was not restarted or altered. The full [implemented and remaining checklist](STRATA_IMPLEMENTED_AND_REMAINING.md) includes every numbered backlog item and distinguishes implementation from installed/hardware acceptance.
