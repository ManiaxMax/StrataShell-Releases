# STRATA Shell 1.0.7

Stable promotion of the tested Preview motion, multi-monitor, material, screensaver, and first-party-app work. This release keeps STRATA's wallpaper-driven glass identity while reducing expensive visual work during movement and making window transitions feel deliberate instead of abrupt.

## Smoother desktop motion

- Synchronizes workspace slides, slot swaps, and desktop-view changes across native and first-party STRATA windows.
- Fades tiled windows out of their old placement and into their settled placement while withholding the active-window highlight until geometry is stable.
- Suspends expensive frost, bloom, and animated-surface work during interactive motion, then restores the complete material after settlement.
- Improves floating drag and resize tracking without applying tiled-window fades to floating windows.
- Makes widget expansion and wallpaper transitions use the same prepared, bounded visual pipeline.

## Display and input reliability

- Strengthens directional focus and move routing across slots, workspaces, and multiple monitors.
- Preserves half-filled workspace slots and current-monitor focus ownership during cross-monitor handoff.
- Improves modifier recovery after screensaver dismissal and elevated-process transitions.
- Keeps adaptive top/bottom and left/right layouts responsive from compact displays through 4K workspaces.

## Theme, glass, and screensaver

- Adds independent Frosted Glass Blur targets for STRATA surfaces and widget cards, plus palette-wide theme bloom.
- Keeps the presented wallpaper, extracted palette, frosted backdrop, and desktop crossfade synchronized.
- Restores bounded screensaver rendering and theme-aware vector effects with stable frame pacing.
- Ships the approved current profile as the privacy-safe fresh-install baseline: 1.15 vibrancy, 50% glass, 50% blur, 40% bloom, 80% eligible-app opacity, shadows off, and a 7 px / 50% surround.
- Opens STRATA Files in Details view with hidden files and thumbnail previews enabled.

## Stable package

- `StrataShell-Setup-1.0.7-win-x64.exe`: full self-contained Setup and uninstaller.
- `StrataShell-1.0.7.zip`: complete self-contained updater and portable bundle.
- `RELEASE_MANIFEST.json`, `VALIDATION_REPORT.json`, individual Setup checksum, and unified `SHA256SUMS.txt`.
- Existing installations retain their personal settings; only fresh profiles receive the new portable defaults.
