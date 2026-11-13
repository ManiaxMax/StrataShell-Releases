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

## October 28 Beta: native dragging rollback and stationary glass candidate

The owner rejected Beta 20261028-113224 after blurred STRATA windows resisted title-bar dragging. Beta 20261028-162332 removes its movement scheduler and rectangle rewrite, restoring unchanged native movement proposals. It also presents the existing blurred wallpaper through a stationary, nonactivating, click-through native surface immediately beneath the moving app. Only that surface's clip follows movement; the bitmap stays anchored at the monitor origin.

The shared material owner handles release, cancellation, blur-disable, hide/close, monitor and texture changes, and fallback. Blur quality, tint and asymmetric corners are retained. Source and isolated checks cover Files, Browser and Settings in Light/Dark, proposal reversals, surface lifetime and clipping at representative scales and negative monitor origins. These checks do not establish physical DWM pixels, pointer smoothness or mixed-monitor handoff; the reported issue remains open pending installed acceptance.

Engine remains 1.0.25. The lightweight update omits runtime binaries, wallpapers and full Setup. Publication does not install, activate or restart the running shell.

## October 28 Beta: constructor-time glass startup correction

Beta 20261028-162332 had a confirmed Rail/Dock startup regression: native backdrop setup attempted to access an HWND before WPF had created it, producing “Hwnd of zero is not valid.” Beta 20261028-171628 defers native hook attachment until SourceInitialized, avoids duplicate attachment or forced handle creation, and cleans up deferred callbacks when an unshown window closes.

Constructor-time material setup and cleanup now run in the quiet release/install self-test. Private-desktop checks exercise repeated pre-HWND material application, combined/Rail/Dock construction and Show in Light/Dark, deferred native dragging and close-before-show, alongside existing glass regressions. Actual installed startup, physical drag smoothness and mixed-monitor acceptance remain pending.

Engine remains 1.0.25. This signed lightweight update contains no runtime binaries, wallpaper library or full Setup. Publication does not install, activate or restart the shell.

## November 8 Beta 20261108-165419

Beta 20261108-165419 builds on the October 28 rollback, 20261028-182220. Windows now clip the covered portion of desktop widgets, leaving the exposed portion visible and interactive. The screensaver requires an input-ready desktop and resets its idle countdown after dismissal or desktop transitions.

Wallpaper reveals and glass crossfades reuse drawing resources and follow display cadence while preserving visual quality. Dock and Rail reserve space from their resting edges, and window layout clears stale insets from prior positions. Docked widget columns use balanced margins. The Active Monitor caption aligns with the workspace digits, and Stylish Audio Spectrum honors the selected Bands mode.

Isolated tests cover these changes. Cold-boot PIN entry, physical input, lower-powered hardware and mixed-monitor presentation need installed testing. The signed lightweight update uses Engine 1.0.25.

## November 8 Beta 20261108-181448

Departing windows keep their original opacity through the workspace slide in Floating and Tiled modes. STRATA hides them at the offscreen endpoint, then resets their source positions while hidden. Regression guards cover the outgoing and returning paths.

Glass follows native window movement immediately by updating the cached wallpaper crop. Cinematic wallpaper effects skip drawing work where an opaque, device-aligned layer fully covers it. The release records composition timing separately from delivered updates.

Private-desktop checks cover native workspace routes, glass alignment, and wallpaper rendering. Physical testing on the installed desktop is pending for external apps, pointer movement, and mixed displays. The signed lightweight update uses Engine 1.0.25.

## November 8 Beta 20261108-192155

STRATA prepares the saved wallpaper, palette, and glass textures before the Dock, Rail, and widgets first render. This addresses the temporary background visible at sign-in.

While a window moves, widget clipping samples the newest bounds once per composition frame. Exposed widget content stays interactive, and the blur retains its configured quality.

Private-desktop checks cover the first rendered frame, widget clipping, and native movement. First sign-in and physical drag testing on the installed build are pending. The signed lightweight update uses Engine 1.0.25.

## November 8 Beta 20261108-204642

STRATA keeps its composition clock active while a visible, blurred window is being dragged. Native movement still updates the cached wallpaper crop, and the clock stops when the gesture ends.

The app catalog shows Antigravity when Windows discovers it or its executable is present in a supported per-user install location.

Private-desktop checks cover the drag lifecycle, widget clipping, and workspace motion. Physical drag cadence and clean-PC app discovery need installed testing. The signed lightweight update uses Engine 1.0.25.

## November 8 Beta 20261108-220637

During eligible window drags, STRATA copies the latest wallpaper crop into a reusable bitmap. The existing brush handles resizing, monitor-edge crossings, and other fallback cases.

Private-desktop checks cover crop freshness, brush restoration, widget clipping, and workspace transitions. Physical drag smoothness on the installed build remains open. The signed lightweight update uses Engine 1.0.25.

## November 9 Beta 20261109-092645

During eligible blurred window drags, STRATA reuses the rendered foreground and keeps the wallpaper layer live. Text and theme changes still refresh the foreground. Windows with native child surfaces and unsupported display configurations use the standard renderer.

Private-desktop checks cover matching Light and Dark pixels, content updates, and cache cleanup. Physical drag smoothness, mixed-DPI behavior, and the reported Browser delay still need installed testing. The signed lightweight update uses Engine 1.0.25.

## November 9 Beta 20261109-112728

Wallpaper crop copies retry only when the bitmap is busy. Audio Spectrum stops requesting frames after its animation settles to silence and wakes when sound returns.

The custom Super + Ctrl resize path places managed windows after the active render pass and uses the latest requested bounds. Private-desktop checks cover these paths, widget response and workspace transitions. The reported wallpaper lag and ordinary corner-resize artifacts still require installed verification. The signed lightweight update uses Engine 1.0.25.

## November 9 Beta 20261109-123658

This documentation update records continued wallpaper drag lag in Files and Browser and progress on an isolated native presentation prototype. The signed package carries the production code from Beta 20261109-112728 and uses Engine 1.0.25.

The prototype is still in isolated testing. Displayed drag timing, corner-resize repaint, and physical mixed-DPI behavior remain open.

## November 9 Beta 20261109-133719

Rail status icons now center their painted artwork, including on narrow side rails. Desktop, widget, and Dock menus align their icons and labels in compact rows.

The desktop reuses widget clip geometry during window movement. Frost fades share one rendering callback per UI thread and keep their own timing and frame limits. Private-desktop checks cover placement, menus, clipping, and fade completion. Installed drag smoothness, corner-resize repaint, and physical mixed-DPI behavior remain open. The signed lightweight update uses Engine 1.0.25.

## November 9 Beta 20261109-150031

A same-monitor drag updates the wallpaper crop immediately and skips an extra geometry refresh when the backdrop is current. Resizing and display changes still trigger reconciliation.

Widget clipping uses one screen-to-layer coordinate transform per update, with the standard WPF conversion for fractional coordinates or unsupported transforms. Private-desktop checks cover crop accuracy, resize handling, and exposed widget pixels and hit testing. Physical drag smoothness, mixed-DPI behavior, and corner-resize repaint remain open. The signed lightweight update uses Engine 1.0.25.

## November 9 Beta 20261109-151715

When the wallpaper bitmap is busy during a drag, STRATA uses the live blur crop at the current window position. It retries the bitmap copy without blocking the drag and reuses the completed crop when the window returns to those bounds.

Private-desktop checks cover the fallback, recovery, and matching Light and Dark pixels. Installed drag smoothness and corner-resize repaint remain open. The signed lightweight update uses Engine 1.0.25.

## November 11 Beta 20261111-113849

Repeated live wallpaper paths share one player across the desktop, Sphere, and Studio. Playback requests pause and release independently. Widget frost and shadows skip repeated coordinate work when layout is stable, and telemetry updates its selected values directly.

Text fields align hints, carets, and typed text through the shared templates. Zen Browser and Medal join the optional application icon set, and desktop menu actions gain distinct icons.

A busy drag bitmap keeps the live crop selected until a new native position requests another copy. Private-desktop tests cover playback sharing, pixels, input geometry, widget layout, and icons. Installed GPU load, physical input, mixed-DPI behavior, wallpaper drag smoothness, and corner-resize repaint remain open. The signed lightweight update uses Engine 1.0.25.

## November 11 Beta 20261111-143211

Widget shadows retain bounded output separately from text, frost, and live video. Spectrum drawing resources and standard telemetry definitions are reused, and identical rendered levels skip redundant updates.

Private-desktop tests compare pixels, resource ownership, live theme changes, playback, and lifecycle cleanup. Allocation and CPU work measurements describe the tested scenes. Installed responsiveness and physical GPU frame pacing remain open. The signed lightweight update uses Engine 1.0.25.

## November 11 Beta 20261111-191018

Widget shadows retain their final composition, including blur, exclusion masks, and viewport clipping. Text, frost, live video, column opacity, and application clipping continue through their existing rendering paths.

Private-desktop checks compare native pixels, effects, content changes, fallback cases, and resource cleanup. A local replay comparison did not reproduce the reported stall; acceptance with the original expensive live file and affected compositor remains open. The signed lightweight update uses Engine 1.0.25.

## November 12 Beta 20261112-185902

Widget blur and final shadow caches can now retain their output when the shadow plane starts at a fractional screen coordinate. Texture bounds align to physical pixels, with compensation inside each tile to preserve the original geometry and shadow phase.

Private-desktop tests cover the reported laptop origin and scale, Light/Dark, viewport scrolling, reuse, fallbacks and lifecycle cleanup. The original video and widget-on/off comparison still need repeating on the affected laptop. Wallpaper-switch stalls remain a separate investigation. The signed lightweight update uses Engine 1.0.25.

## November 12 Beta 20261112-193947

Overlapping applications no longer disable the final widget shadow caches. Application exclusion clips stay above the retained native-pixel output, allowing moving windows to reuse unchanged shadow resources.

Private-desktop checks compare exact pixels under rectangular, rounded and elliptical fractional clips, translucent columns and the shared application-exclusion geometry. Affected-laptop playback and compositor acceptance remain open. The signed lightweight update uses Engine 1.0.25.
