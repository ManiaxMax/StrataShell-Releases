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

## Latest Beta: wallpaper effects, clock format and widget controls

Beta 20261026-203751 adds eight cinematic wallpaper effects for still and live wallpapers, cached Wallpaper Studio browsing, and protected Recycle Bin deletion. Date + Time shares a 12/24-hour preference with the widget, bar and calendar panel. Settings exposes global and individual widget appearance controls. Audio Spectrum adds smoother attack/release, capture-buffer ownership protection and wider radial rendering.

Engine remains 1.0.25. The update contains Shell, Launcher, Maintenance, safe defaults and installer support, with no bundled runtime, wallpaper library or full Setup. Source, localization and isolated UI checks cover this candidate; installed GPU/DWM, codec presentation, mixed-DPI and physical input/playback acceptance remain pending. Publication does not install, activate or restart the running shell.
