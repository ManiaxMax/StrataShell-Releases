# Phase 3: cross-computer reliability and fullscreen priority

Date: 2026-09-04. Source base: `0e81a03` (Phase 2), branch `reliability/phase-3-cross-computer` in `C:\Projects\StrataShell`. Changes are local and uncommitted. This implements audit R2–R7 plus the requested fullscreen-game behavior. Installed hardware acceptance remains open under STRATA-039.

## Delivered behavior

| Area | Change |
| --- | --- |
| Fullscreen games | Native monitor-filling games bypass tiling, opening fades, material transparency and background dimming. STRATA removes its opacity effect and temporarily holds supported constant-alpha layers opaque, restoring original attributes afterward. Surround, active border, rail/dock and widgets yield on every display covered by the game; suppression follows fullscreen moves between monitors; launcher, quick panels, app switcher and ordinary OSDs are suppressed. Settings refresh and delayed Wi-Fi panels cannot resurrect them. Minimized games are not forcibly restored by the tiler. Fullscreen also defers the visual screensaver. |
| Display continuity | Monitor identity replaces transient handles for matching. Reconciliation after docking, resume and session reconnect retains widget hosts, paused/running timer state and AI sessions, rebinds gesture input, and uses per-monitor widget occlusion and stage envelopes. Disconnected-display windows move onto surviving workspaces and their saved floating/restore rectangles are scaled and clamped. A destination exceeding twenty tiled apps falls back to Floating mode to preserve access. |
| State migration | Schema 38 preserves appearance/app settings, retires process suppression and adds up to sixteen display-preference records keyed by hashed identity. No HWNDs or app content are persisted there; installer defaults contain an empty dictionary. |
| Small displays | Settings switches to compact navigation below 920 DIPs. Common dialogs and first-party workspace windows fit the current work area with smaller minimum dimensions when necessary. Dialog actions wrap and remain separate from scrollable long content; title bars remain reachable after display changes. Fullscreen geometry is exempt. |
| Windows preferences | STRATA no longer raises Windows screen-saver, display-off or sleep timeouts, nor periodically terminates Windows component processes. Windows lock/power settings take precedence. New backups do not claim ownership of those idle choices; legacy restoration is guarded against overwriting later preferences. |
| Recovery | Three unexpected exits within ten minutes, including exit zero, trigger the existing Explorer fallback. Deliberate restart/Explorer requests are excluded. An independent bootstrap window offers Keep Waiting or confirmed temporary Explorer recovery after a sustained UI stall; startup/resume get 60 seconds of grace, ordinary stall threshold is 30 seconds, and lock/suspend defer it. Missing heartbeats alone never terminate the shell. Recovery stops the shell process, preserving external application processes. Unexpected dispatcher failures notify bootstrap instead of silently claiming recovery. |
| Wi-Fi | Locale-independent native WLAN enumeration replaces English command-output parsing. Adapter GUIDs, SSID bytes, profile names and security types stay distinct. Supported new open/WPA2/WPA3 Personal/OWE profiles enroll directly in memory; no temporary password XML file is written. Existing Windows profiles cover enterprise, hidden and unsupported enrollment types. Unknown security never becomes open. Connection attempts run off the UI thread, serialize, support cancellation and verify connection status. Permission/service failures and captive-portal limitations are reported. |

The monitor identity comes from the Windows device-interface API and falls back to an unambiguous connector/geometry match when a driver replaces its identifiers. This follows [EnumDisplayDevices documentation](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-enumdisplaydevicesw). Secondary widget sessions are retained hidden until reconnection or shutdown; they can retain memory and background work. The primary widget session follows the primary display.

Wi-Fi state uses the native [WLAN network structure](https://learn.microsoft.com/en-us/windows/win32/api/wlanapi/ns-wlanapi-wlan_available_network) and [connection API](https://learn.microsoft.com/en-us/windows/win32/api/wlanapi/nf-wlanapi-wlanconnect). Windows can deny network-list access under its [Wi-Fi location-permission rules](https://learn.microsoft.com/en-us/windows/win32/nativewifi/wi-fi-access-location-changes). STRATA does not bypass or change those permissions.

## Local verification

- Release shell build: zero warnings/errors. Maintenance project built and exercised through its isolated probe.
- Standard quiet suite: **136/136 passed**, including twelve new Phase 3 checks. The opt-in visual-fixture run passed **137/137**.
- Private desktop suite: **146/146 passed**. Covers actual native window bounds and alpha, fullscreen enter/exit/minimize, surround and rail hiding, notification suppression, settings/mode changes, retained widget HWND/AI host/timer, plus existing app/dock/widget regressions. Final count is recorded in `artifacts/phase3-reliability/desktop/report.json`.
- Maintenance: **7/7 passed** using a unique test registry prefix and temporary data root. Cases include fresh backups, legacy 900-second residue, later user changes, Never, old schema upgrade and backward-compatible backup deserialization. No production idle or power settings were written.
- Eighteen unshown real Settings, dialog and recovery renders cover Light/Dark at 100%, 150% and 200%. Compact Settings, long-dialog actions and the recovery choices were visually inspected. Evidence is under `artifacts/phase3-reliability/visuals`.
- The private-desktop runner now rejects exits without a completion report. Its animation check samples actual movement instead of assuming a single 120 ms sample catches a frame on a hidden desktop.
- Test artifacts: `self-test.json`, `desktop/report.json`, `maintenance/report.json` beneath `artifacts/phase3-reliability`. Reproduce with quiet self-test, `scripts/Test-StrataDesktopMode.ps1` and `scripts/Test-StrataPhase3Maintenance.ps1`.

The fullscreen probes use a borderless WPF window and a plain native HWND with controlled constant-alpha attributes. They are not DirectX games. The heartbeat probe uses real named events and deliberately stalls only the test dispatcher. A complete installed bootstrap fault/recovery session has not been induced. Native Wi-Fi structure/security/credential tests use fixtures; this pass did not connect, disconnect or scan the user's actual network.

## Installed acceptance still required

- [ ] MX450 Zenbook and Iris Xe at 60 Hz: move Browser repeatedly between screens, including different DPI, with secondary display enabled/disabled; check shell responsiveness and resumed browser input/video.
- [ ] Real borderless and exclusive-fullscreen games: confirm solid image and no surround, rail/dock, widgets, active border or OSD over the game; try settings updates, Alt+Tab, minimize/restore, exit and switching displays. Check mixed refresh, HDR and elevated games where available. No claim of measured gaming FPS improvement is made.
- [ ] Dock/undock with apps and dialogs on each display; test negative origins, portrait and ScreenPad-sized work areas, 100/125/150/175/200% DPI, minimized/maximized windows, multiple workspaces and timer/AI/media continuity. Confirm controls remain reachable after reconnect/resume.
- [ ] On a disposable test account/session, stall the installed shell and exercise Keep Waiting/recovery; force repeated delayed crashes and confirm Explorer fallback. Confirm lock/sleep/resume causes no false takeover.
- [ ] Non-English Windows and available adapters: saved profile, WPA2/WPA3, OWE, unsupported/hidden/enterprise profile, wrong password, cancel, removed adapter and denied Location access. Verify captive-portal status without treating Wi-Fi association as Internet access.

Windows secure UI and explicitly requested emergency recovery remain available above a game. STRATA cannot control a game's own rendering, driver behavior, third-party overlays or incompatible per-pixel-alpha surfaces. Foreground fullscreen can suppress shell chrome even when the window cannot be adopted; the shell does not escalate privileges to alter it.

Legacy custom idle floors are not guessed or automatically undone during update. Guarded recovery/uninstall handles the known old 900-second residue; other existing values can be reviewed in Windows Settings.

## Delivery boundary

The running shell was verified at installed release `20260904-224303`. This pass did not install, activate, restart, sign out, publish or push. Signing and update trust from Phase 1 remain unchanged. A new Preview release can be built and distributed from the tested source when publication is requested; the current installed release does not contain these changes.
