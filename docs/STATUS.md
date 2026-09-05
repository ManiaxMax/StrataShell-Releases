# Release and testing status

Updated September 5, 2026. Documentation describes current Preview development; the source `main` branch and published Stable packages can predate it.

## Published packages and current work

- The latest published Stable package checked for this documentation pass was **1.0.12**. Always consult the [release feed](https://github.com/ManiaxMax/StrataShell-Releases/releases) for newer packages and their actual contents.
- **Phase 5** covers code review, installer defaults, documentation, repository branding and the reported normal-maximize regression. Updating a README does not publish application binaries or install a new shell.
- The preceding tray, sound, widget, first-party app and native-settings work exists in Preview source. Automated checks and physical acceptance are separate; not every hardware or desktop interaction has been observed.
- Fresh-install Phase 5 defaults apply only to the first release that packages the updated template. Older packages retain their own defaults.

Normal maximization is being corrected at the native per-monitor work-area level for Explorer-free shell sessions. Custom-chrome maximized apps stay separate from true fullscreen games. The running pre-Phase-5 release does not include this change; physical acceptance on the updated candidate remains pending.

## Fresh installations versus updates

New release installs without an existing settings file receive Balanced quality, Floating mode and the [approved widget layout](SHIPPED_DEFAULTS.md). Clock/Time and YouTube start locked expanded. Setup and portable installation preserve an existing profile, including older and partially populated profiles. Shell startup does not apply the release template. Reset Settings remains an explicit user action.

## Evidence and remaining acceptance

The latest local candidate passed 555 desktop checks, 327 application checks, 144 self-tests and 22 fresh-install/preservation checks. These are source-build results, not proof of installed behavior.

The development checks cover settings migration/preservation, first-party document lifetimes, picker ownership, display rollback, sound controls, notification-icon callbacks, widget expansion and window/workspace policies. They use isolated Windows desktops and test profiles where appropriate.

Live Sound Settings output selection, master volume and mute/unmute have been exercised, with the original visible configuration restored. Additional sound dropdown/focus corrections were subsequently made in source.

Still awaiting complete physical acceptance:

- Antigravity/Tailscale Open behavior, including closed-window and native-menu paths;
- both locked widgets visibly expanded before first hover after startup;
- the reported Text/Snip close, picker outline, Task Manager overlap/transparency, Floating maximize and multi-monitor workspace sequences on the exact candidate;
- microphone operation, per-app audio changes, real Bluetooth pairing and Wi-Fi connection/radio changes;
- display rollback/hotplug, supported touchpad controls, startup selections and power/time changes on representative hardware;
- compositor previews, minimize/workspace animations and synchronized High Quality wallpaper/frost transitions.

The live capture tool failed during the latest check, so animation appearance and the shell bar were not marked verified from automated screenshots. Smoothness is not guaranteed across all machines or workloads.

## Capability limits

- Windows per-monitor DPI is read-only. STRATA's own interface scale is separately adjustable.
- Vendor-only display/input controls, historical battery usage, immediate Energy Saver switching and Windows Update installation/scheduling are outside the implemented controls.
- Tray publishers choose their own actions. Open restores a tracked window; otherwise STRATA requests native controls, with a guarded activation fallback. A closed application may require its own menu's Open command.
- Windows owns UAC, sign-in, protected consent, drivers and secure surfaces. Hardware support depends on those interfaces.
- Windows publisher/Authenticode signing is deferred. STRATA's signed update inventory is a separate existing integrity mechanism.
- The previously named laptop-specific investigations remain outside the active scope.

Detailed developer evidence is maintained in the private source repository. Published release notes remain the authority for an individual download.
