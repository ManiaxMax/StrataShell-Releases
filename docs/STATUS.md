# Current release status

Updated October 25, 2026 for [Stable 1.0.25](https://github.com/ManiaxMax/StrataShell-Releases/releases/tag/v1.0.25). The [release feed](https://github.com/ManiaxMax/StrataShell-Releases/releases) confirms public availability.

Stable 1.0.25 promotes Beta 20261025-162236. It offers self-contained Windows 11 x64 Setup and portable packages with matching Shell, Launcher, Maintenance and Setup versions, the .NET 10 Desktop runtime, safe defaults and exactly 24 approved 4K glass wallpapers. See the [1.0.25 release notes](RELEASE_NOTES_1.0.25.md) for changes and limits.

The prerelease channel is **Beta**, formerly Preview. Older installed builds retain the Preview label until updated; saved selections remain compatible. The [October 25 Beta](https://github.com/ManiaxMax/StrataShell-Releases/releases/tag/20261025-235549) is a lightweight update requiring a compatible Stable installation or registered .NET runtime. The external STRATA Launcher checks Stable only.

Optional automatic Stable update checks default off, offer Every Launch or Weekly on Sunday, and never automatically download or install. Existing installation terms and explicit acceptance remain required.

STRATA remains experimental alpha software. Source, package and isolated checks do not establish installed physical acceptance. Multi-monitor input/DPI, hardware/GPU paths, screen-reader behavior and native-speaker translation review remain open. Publishing an update does not install, activate or restart the running shell. Update archives require STRATA signatures; Windows publisher signing is not configured.

## October 25 Beta launch-review update

Beta 20261025-185858 adds Launcher, Setup and Uninstall localization across 25 catalogs; bounded update requests, complete runtime-bridge receipt checks and validated startup imports; wallpaper preparation and Focus Timer refinements; and private Browser Light-theme correction. It also strengthens acceptance-report validation. Engine remains 1.0.25. Setup changes are source-tested here and will ship with a separately approved full Stable package; Beta contains only the lightweight update unit.

The source builds and 313 quiet self-tests pass. The broader launch review retains an unresolved Sphere frame-rendering test on an inactive private desktop; physical installed rendering acceptance remains open. Publication does not repair activation policy, install, activate or restart STRATA.

## October 25 Beta: workspace motion, screensaver pacing and Task Manager

Beta 20261025-235549 includes every pending workspace-return, screensaver and Task Manager change. Returning app windows remain visible through intermediate slide frames, including recovery from a delayed hide. Screensaver scheduling follows each display's reported refresh rate and the composition callbacks actually available, preserving all 30 animations and particle density. Task Manager uses a simpler process column list and opens executable locations in STRATA Files.

These changes have source and isolated-test coverage. Actual GPU frame rate, physical workspace presentation and installed input remain separate acceptance. Publishing this lightweight Beta leaves the running installation and Stable channel unchanged.

## October 26 Beta: wallpaper effects, clock format and widget controls

Beta 20261026-203751 adds eight cinematic wallpaper effects for still and live wallpapers, cached Wallpaper Studio browsing, and protected Recycle Bin deletion. Date + Time shares a 12/24-hour preference with the widget, bar and calendar panel. Settings exposes global and individual widget appearance controls. Audio Spectrum adds smoother attack/release, capture-buffer ownership protection and wider radial rendering.

Engine remains 1.0.25. The update contains Shell, Launcher, Maintenance, safe defaults and installer support, with no bundled runtime, wallpaper library or full Setup. Source, localization and isolated UI checks cover this candidate; installed GPU/DWM, codec presentation, mixed-DPI and physical input/playback acceptance remain pending. Publication does not install, activate or restart the running shell.

## October 26 Beta: widget caching during wallpaper transitions

Beta 20261026-220019 temporarily caches the widget content above animated frost during wallpaper transitions, reducing repeated redraw work while preserving effects, timing, full image quality and live control/theme updates. The hardware-gated caches follow scale, visibility and transition lifetime, respect external cache ownership, and release at rest or close. Existing live-video caching remains in place.

Engine remains 1.0.25. Source and isolated checks cover cache ownership and cleanup, Light/Dark changes, all eight cinematic effects and related wallpaper regressions. Physical installed GPU/DWM smoothness and mixed-monitor frame pacing remain pending. This lightweight Beta omits runtime binaries, wallpapers and full Setup; publication does not install, activate or restart the running shell.

## October 28 Beta: glass alignment during window movement

Beta 20261028-101152 corrects a shared glass timing defect: Windows moved the window before the wallpaper crop updated on a later rendering callback. Native move/resize notifications now update the existing texture crop immediately while full monitor/texture refreshes remain coalesced. Monitor changes refresh immediately, and closed or suspended materials skip movement updates. Blur quality and texture reuse are preserved.

Source and isolated checks cover Files, Browser and Settings in Light/Dark, immediate crop alignment, full-refresh limits, shell drag commits, hide/close cleanup, live playback and disabled blur. Physical installed GPU/DWM smoothness and mixed-DPI monitor transfer remain pending. Engine remains 1.0.25; the lightweight update contains no runtime binaries, wallpaper library or full Setup. Publication does not install, activate or restart the running shell.

## October 28 Beta: title-bar movement pacing candidate

The owner reports that glass jitter persisted on another PC after the preceding crop-timing update. Beta 20261028-113224 is a follow-up candidate for still wallpaper and ordinary title-bar dragging of blurred STRATA apps in Floating mode; the reported issue remains open.

Native movement proposals now share a frame-paced window-placement and wallpaper-crop commit. The newest proposal wins, release flushes before edge snapping, and cancellation, sizing, hide/close and display migration remove pending placement. Blur-off and third-party movement retain their existing paths; blur quality is unchanged.

Source and private-desktop checks cover Files, Browser and Settings in Light/Dark, direction reversals, release/cancel cleanup and real native movement-loop progress. WPF callback timing does not prove displayed frame continuity. Physical dragging, Escape, edge snapping and mixed-DPI acceptance on the affected PC remain pending. Engine remains 1.0.25, and this lightweight Beta contains no runtime binaries, wallpaper library or full Setup. Publication does not install, activate or restart the running shell.

## Latest Beta: native dragging rollback and stationary glass candidate

The owner rejected Beta 20261028-113224 after blurred STRATA windows resisted title-bar dragging. Beta 20261028-162332 removes its movement scheduler and rectangle rewrite, restoring unchanged native movement proposals. It also presents the existing blurred wallpaper through a stationary, nonactivating, click-through native surface immediately beneath the moving app. Only that surface's clip follows movement; the bitmap stays anchored at the monitor origin.

The shared material owner handles release, cancellation, blur-disable, hide/close, monitor and texture changes, and fallback. Blur quality, tint and asymmetric corners are retained. Source and isolated checks cover Files, Browser and Settings in Light/Dark, proposal reversals, surface lifetime and clipping at representative scales and negative monitor origins. These checks do not establish physical DWM pixels, pointer smoothness or mixed-monitor handoff; the reported issue remains open pending installed acceptance.

Engine remains 1.0.25. The lightweight update omits runtime binaries, wallpapers and full Setup. Publication does not install, activate or restart the running shell.
