# Shipped Strata defaults and privacy scrub

The installer packages self-contained Strata shell builds with fresh-install defaults derived from `installer/Defaults/settings.safe.json` (`SchemaVersion 23`). Existing users keep their own `settings.json` during install and update with automatic schema migration.

## Retained

- appearance mode, vibrancy, opacity, blur, bloom, borders, gaps, animation, shadows, and transparency;
- workspace count, center-stage sizing, file-view preferences, top-rail visibility, screensaver timing, and widget layout/behavior;
- theme synchronization, wallpaper-grid behavior, accessibility toggles, update channel, and other non-identifying UI preferences;
- Fahrenheit and clock-format preferences without retaining any place name.

## Replaced with portable installer values

- wallpaper library, active wallpaper, and last Light/Dark wallpaper paths point to the installer-owned per-user wallpaper library;
- only the existing `Strata_*` files from `WallpapersLight` and `WallpapersDark` are packaged (20 in each folder); no other local images are copied;
- the portable default pair is `Strata_Green_Black_Abstract_4K.png` and `Strata_Green_White_Abstract_4K.png`;
- wallpaper recents are an empty array;
- the audio-spectrum source is reset to the default device rather than a machine-specific endpoint;
- the timer reminder is the generic text `Focus session complete`;
- the AI Command widget is enabled with portable defaults (`codex` provider, `C:\Projects` working directory).

## Removed or disabled

- weather ZIP code, city, region, coordinates, and other location text;
- AI session identifiers, conversation history, and login/account assumptions;
- passwords, tokens, cookies, credentials, account IDs, machine IDs, user IDs, and machine/user-specific paths;
- launcher/search history, recent files and wallpapers, monitor device identifiers/layout fingerprints, and network/device selections;
- logs, diagnostics, caches, lock-screen cache files, recovery records, and the existing installation record.

No current `settings.json`, log, cache, session database, browser data, or recovery backup is embedded in the installer. The allowlisted template is `installer/Defaults/settings.safe.json`.
