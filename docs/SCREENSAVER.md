# STRATA screensaver

The integrated screensaver is the production host for the approved STRATA logo preview. It is not a Windows `.scr` registration and does not depend on Explorer.

## Runtime contract

- Enabled by default after 300 seconds of global keyboard/mouse inactivity.
- One opaque, borderless, topmost `ScreensaverWindow` is created for every physical display.
- Shell-owned topmost overlays, including the active-window signal frame, are suppressed before the first screensaver surface appears and restored only after every saver window closes.
- Automatic activation defers while Windows reports presentation mode or full-screen Direct3D activity. The generic `Busy` notification state is intentionally ignored because Windows can leave it asserted during ordinary desktop use.
- STRATA Command and the Screensaver Settings pane start it immediately unless a visible fullscreen application is active. Fullscreen also defers automatic activation.
- Mouse movement beyond the startup guard, mouse buttons, wheel input, touch/stylus input, or any keyboard key dismisses every surface with a short fade.
- The global keyboard gate consumes the dismissal key and its paired release so input does not leak into the application underneath.
- Session lock, suspend, display-topology changes, and shell shutdown close or rebuild the surfaces safely.

## Visual contract

`StrataSaverSurface` renders 30 distinct modes. The original catalog remains—Signal Bloom, Horizontal Drift, Chromatic Split, Scan Materialize, Orbital Lock, Digital Rain, Prism Wipe, Shard Reassembly, Pulse Matrix, Ghost Trails, Logo Supernova, Thunder Seal, Firework Assembly, Gravity Rebirth, and Laser Etch—and is joined by Signal Constellation, Vector Fracture, Aurora Weave, Helix Transmission, Quantum Lattice, Plasma Cascade, Spectral Echo, Data Vortex, Neon Monolith, Particle Forge, Temporal Slice, Solar Flare, Hologram Fold, Circuit Bloom, and Event Horizon.

The production mark is no longer a raster mask. `StrataBrandGeometry` is the shared source for the uppercase three-bar logo shown in Settings → About and the screensaver. `LogoAssets` composes that mark, uppercase `STRATA` wordmark, and system-signal descriptor as frozen WPF vector drawings. The same geometry remains sharp at every supported resolution, and particle density grows from a bounded 720p baseline through a denser 4K field.

The background uses layered aurora ribbons, a drifting spectral mesh, angular contour frames, and narrow theme-colored streaks rather than generic radial circles and broad white sweep bars. Runtime color is projected from `ThemeService.CurrentPalette`; the screensaver never extracts or owns a second wallpaper palette. The light presentation uses a darker charcoal-mineral canvas and more saturated signals instead of full white or washed gray while remaining visibly lighter than the dark presentation.

Every catalog boundary keeps one continuous ambient field while the outgoing and incoming logo treatments use a 1.6-second smoother-step crossfade, so background geometry, particles, and logo effects never hard-cut or flash. The logo uses layered chromatic ghosts, broad radial phosphor clouds, moving scanlines, and a near-black CRT edge mask for deliberately strong bloom without rasterizing the 4K mark. Scanlines are a frozen tiled CRT pattern rather than hundreds of per-frame line primitives, and the transition never redraws two complete 4K scenes.

Animation time comes from one shared high-resolution timeline for all screensaver windows. Each surface accumulates WPF composition samples but invalidates the complete vector/particle scene at no more than 60 Hz. Mixed-refresh monitors therefore show the same animation phase and speed, while DWM still presents each surface at that monitor's native cadence. This avoids rebuilding the full display list 144–240 times per second without rasterizing or lowering the native-resolution mark.

Reduced Motion removes the high-motion wave/particle paths and uses a calmer fixed cycle. High Contrast uses a black background and a crisp foreground logo.

## Windows idle settings

STRATA reads Windows screen-saver, display-off, standby and managed-lock settings for status only. It does not extend timeouts, change the active power plan or disable lock policy. Windows settings take precedence; the STRATA screensaver is a visual surface, not a secure lock screen.

The old 900-second floor is retired. Updating does not guess which existing Windows values the user intended. New installation backups do not claim ownership of idle settings. During requested recovery/uninstall, a legacy backup restores a known pre-floor nonzero value only when the current value is still 900; later changes and Never are preserved. Older custom floor values require review in Windows Settings.

## Verification

The noninteractive suite validates settings defaults, all 30 unique catalog entries, shared vector geometry, resolution-aware particle tiers, a single ambient transition pass, bounded bloom passes, tiled CRT scanlines, restrained light/dark luminance, crossfade easing, the 60 Hz vector render ceiling across mixed compositor rates, normal and wrapped last-input timing, generic-busy versus presentation deferral, keyboard dismissal consumption, and read-only timeout inspection. Visual, sustained-frame-rate, and physical-input acceptance should be performed on the installed build with the manual preview route before relying on unattended activation.
