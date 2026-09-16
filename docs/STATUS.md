# Release and testing status

Updated September 5, 2026. This describes current local development. Published packages retain the features and runtime identified in their own release notes.

## Current implementation

Phase 5 defaults, documentation and normal-maximize changes reached installed Preview `20260905-135348` on .NET 9.0.19. A later local candidate now targets **.NET 10** across Shell, Launcher, Maintenance and Setup, with updated runtime checks and packaging scripts. It has not been published or installed in this pass. Stable 1.0.12 was the last published Stable checked; consult the [release feed](https://github.com/ManiaxMax/StrataShell-Releases/releases) for actual downloads.

The later candidate adds native AC/battery power modes and plan selection, a battery tray menu, automatic temporary Low effects during power saving, 14-day battery/AC usage history, hardware inventory, aligned System Info cards, settings scroll retention, YouTube startup restoration, read-only-cache uninstall cleanup, the tray recovery correction and a wallpaper-hang mitigation.

**Stable bundles the .NET 10 Desktop runtime. Preview stays lightweight.** A .NET 10 Preview needs a compatible registered Desktop runtime or an exact compatible Stable runtime bridge. An older .NET 9 installation cannot supply that bridge.

## Fresh installations and existing preferences

Fresh release installations receive Balanced quality, Floating mode and the [approved widget layout](SHIPPED_DEFAULTS.md), with Clock and YouTube locked expanded. Existing profiles are preserved on update, repair and startup. The new automatic Low quality option defaults to enabled when absent; it temporarily changes effective rendering without overwriting the user's saved quality settings.

## Verification and installed acceptance

The local candidate passed 339 application checks, 556 desktop/widget checks, 144 self-tests, 22 default/profile-preservation checks, 11 maintenance checks and 9 isolated runtime/install/uninstall lifecycle checks. The final hardware/native-settings/card-resize run passed 83 checks. These reports overlap in coverage and are not a unique test total. They establish local and isolated behavior, not deployment or universal hardware acceptance.

The owner reports the existing Bluetooth, audio/microphone, Wi-Fi, display and supported input/power controls work as intended. The new power-mode/low-battery transitions, temporary Explorer workflow, YouTube startup fix and exact reported hang sequence still need the new candidate installed. Ordinary maximize/bar boundaries, tray activation, widgets, compositor previews and animations remain subject to physical acceptance on the candidate being tested. Still images do not establish animation smoothness.

The latest reported hang was recorded by Windows as AppHangB1 during a wallpaper/theme change; no blocking stack was available. The new candidate avoids a broad settings rebuild in that path and records the current theme operation if another stall occurs. This is a mitigation pending installed reproduction, not a proven root-cause diagnosis.

## Capability limits

- Per-monitor Windows DPI changes and immediate Energy Saver toggling use direct Windows Settings routes; STRATA's own scale and native Windows power modes are separate controls.
- Native Windows Update availability/history is implemented. Installation and scheduling use Windows Update Settings, with an optional tracked temporary Explorer session that preserves the chosen next-login shell.
- Battery history covers total active AC/battery time, not per-app energy attribution.
- Motherboard/BIOS/RAM/cache inventory is available independently of sensor software. CPU temperature still requires an already-running supported sensor provider; no additional sensor driver is installed. NVIDIA temperatures use the installed driver's NVML interface.
- Vendor-only controls, protected Windows security surfaces and application-defined tray behaviors retain their platform limitations.
- Publisher/Authenticode signing is deferred. The retired laptop investigations remain outside the active scope.

Developer evidence stays in the private source repository. Updating documentation does not publish an application binary.

See [the detailed implementation and remaining-work report](POWER_RUNTIME_FOLLOWUP.md).
