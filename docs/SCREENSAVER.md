# Strata screensaver

The integrated screensaver is the production host for the approved Strata logo preview. It is not a Windows `.scr` registration and does not depend on Explorer.

## Runtime contract

- Enabled by default after 300 seconds of global keyboard/mouse inactivity.
- One opaque, borderless, topmost `ScreensaverWindow` is created for every physical display.
- Automatic activation defers while Windows reports presentation mode or full-screen Direct3D activity. The generic `Busy` notification state is intentionally ignored because Windows can leave it asserted during ordinary desktop use.
- `Super + Shift + S` and the launcher/Settings preview route start it immediately.
- Mouse movement beyond the startup guard, mouse buttons, wheel input, touch/stylus input, or any keyboard key dismisses every surface with a short fade.
- The global keyboard gate consumes the dismissal key and its paired release so input does not leak into the application underneath.
- Session lock, suspend, display-topology changes, and shell shutdown close or rebuild the surfaces safely.

## Visual contract

`StrataSaverSurface` renders the exact approved ten-mode baseline: Signal Bloom, Horizontal Drift, Chromatic Split, Scan Materialize, Orbital Lock, Digital Rain, Prism Wipe, Shard Reassembly, Pulse Matrix, and Ghost Trails.

The logo mask is embedded from `assets/brand/strata-logo-source.png`. Its documented SHA-256 is `1654A7EFEF3A310A37737B9279B425ADC7852D03A8F39AA888B8B892A501677E`. Runtime color is projected from `ThemeService.CurrentPalette`; the screensaver never extracts or owns a second wallpaper palette.

Reduced Motion removes the high-motion wave/particle paths and uses a calmer fixed cycle. High Contrast uses a black background and a crisp foreground logo.

## Windows idle floor

At service start, Strata raises only Windows timeouts that would preempt the visual surface:

- ordinary current-user Windows screen-saver timeout;
- AC/DC display-off timeout;
- AC/DC standby timeout.

The minimum is 900 seconds. A value of `0` (Never) and any value already at or above 900 seconds is preserved. An administrator-enforced `InactivityTimeoutSecs` below the floor is detected and reported under Settings → Screensaver; Strata does not rewrite machine policy.

## Verification

The noninteractive suite validates settings defaults, the effect catalog, normal and wrapped last-input timing, generic-busy versus presentation deferral, keyboard dismissal consumption, and timeout-floor normalization. Visual and physical-input acceptance should be performed on the installed build with the manual shortcut before relying on unattended activation.
