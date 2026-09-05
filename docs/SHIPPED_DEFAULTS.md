# Shipped STRATA Shell defaults and privacy scrub

The next release containing Phase 5 seeds `installer/Defaults/settings.safe.json` only for a brand-new installation without an existing settings file. The profile uses **Balanced quality** and **Floating mode**, with Browser and Files pinned. Setup and portable installation preserve existing profiles; neither updates nor shell startup apply this new layout. Explicit Reset Settings is a separate user action. The schema remains 40 because this is an installer-template change, not a new preference or migration.

| Slot | Left column | Right column |
|---|---|---|
| 0 | Clock / Time — locked expanded | YouTube — locked expanded |
| 1 | Weather | Audio Spectrum |
| 2 | Focus Timer | Performance |
| 3 | Notes | AI Command |
| 4–7 | Unassigned | Unassigned |

All eight widgets are enabled; hover expansion and widget titles are enabled. Five workspaces are configured. Only portable layout and behavior are copied into the template: personal widget content and hardware identities are not included.

## Portable preferences in the template

- appearance mode, vibrancy, persisted material opacity, user-facing transparency, Frosted Glass Blur, bloom, borders, gaps, animation, performance mode, frame rate cap, wallpaper scaling, wallpaper fit mode (`Fill`, `Fit`, `Stretch`), battery saver, and shadows;
- workspace count, center-stage sizing, file-view preferences, top-rail visibility, screensaver timing, and widget layout/behavior;
- theme synchronization, wallpaper-grid behavior, accessibility toggles, update channel, and other non-identifying UI preferences;
- Fahrenheit and clock-format preferences without retaining any place name.

## Replaced with portable installer values

- wallpaper library, active wallpaper, and last Light/Dark wallpaper paths point to the installer-owned per-user wallpaper library;
- Stable packages include exactly `WallpapersLight/STRATA_LOGO_LIGHT.jpg` and `WallpapersDark/STRATA_LOGO_DARK.jpg`; Preview update packages omit wallpapers and preserve the installed library;
- the portable default pair is `STRATA_LOGO_DARK.jpg` and `STRATA_LOGO_LIGHT.jpg`;
- wallpaper recents are an empty array;
- the audio-spectrum source uses the portable Windows system mix rather than a machine-specific endpoint;
- the timer reminder is the generic text `Focus session complete`;
- the AI Command widget is enabled with portable defaults (`antigravity` provider, `C:\Projects` working directory).
- weather uses recognizable normal icons by default; themed STRATA glyphs remain selectable per user.

The packaged visual baseline follows the Balanced desktop profile: 40% glass transparency (0.60 opacity), 40% Frosted Glass Blur (disabled by default for surfaces and widgets), 0% Bloom, 80% eligible-app opacity, compatible window shadows disabled, widget shadows disabled, a 30 px top bar with an always-visible power button, and a 7 px / 40% workspace surround. Wallpaper-derived vibrancy starts at 1.15. STRATA Files starts in Details view with hidden files visible and thumbnail previews enabled.

## Removed or disabled

- weather ZIP code, city, region, coordinates, and other location text;
- AI session identifiers, conversation history, and login/account assumptions;
- passwords, tokens, cookies, credentials, account IDs, machine IDs, user IDs, and machine/user-specific paths;
- launcher/search history, recent files and wallpapers, monitor device identifiers/layout fingerprints, and network/device selections;
- logs, diagnostics, caches, lock-screen cache files, recovery records, and the existing installation record;
- per-display layout preferences (`DisplayLayouts` ships empty); the retired Windows process-suppression flag is disabled.

No current `settings.json`, log, cache, session database, browser data, or recovery backup is embedded in the installer. The allowlisted template is `installer/Defaults/settings.safe.json`.

Schema 40 ships no selected startup apps (StartupApplicationIds is empty). STRATA touchpad workspace swipes are enabled with a threshold of 180. Native display, input, sound, power and clock values are read from Windows; installing STRATA does not apply new hardware defaults.
