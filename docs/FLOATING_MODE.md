# Tiled and Floating environments

New installations start in Floating mode. Existing profiles retain their chosen mode. Switch between Floating and Tiled with **Super + Ctrl + D** or **Settings → UI & Theme → Interface → Window Management Mode**. The choice persists across launches.

## Floating

- Drag a window's title bar to move it and its edges to resize it. Ordinary windows can overlap, minimize, and maximize within the monitor's usable work area.
- Use the bottom dock to launch or restore apps. Browser and Files are pinned on a fresh installation. Running app groups offer previews and individual window actions; pins can be changed from their context menus.
- **Alt + Tab** cycles apps on visible workspaces. Click empty desktop space to minimize visible apps; use the dock or Alt + Tab to restore them.
- Launcher, tray, power, audio, and network panels open above the bottom bar. True fullscreen uses the full display and hides the dock and other shell overlays until fullscreen ends.
- Show Widgets / Hide Widgets controls widget visibility on each monitor. Wallpaper, theme, workspace, and monitor choices remain available.
- Settings offers floating background transparency, desktop-click minimization, and dock height. New installations start at 0% floating background transparency; existing preferences are preserved.

## Tiled

Tiled mode arranges one or two apps in Center Stage. Wide side-by-side, wide top/bottom and Dynamic views provide other layouts; Dynamic can arrange additional tiled apps. Workspace and monitor controls remain available. Hold **Super + Ctrl** while dragging inside a window to temporarily float and move it, or drag its border to resize it. **Super + Ctrl** plus right-click returns a floating window to the tiled layout.

Switching back from Floating restores the top rail and tiled views. STRATA distributes open apps among workspaces and asks you to move or close some if the configured capacity would be exceeded; it does not discard windows.

## Compatibility and release status

Some third-party windows control their own chrome or require opaque rendering. Native fullscreen and elevated applications can have different behavior. See [features and limitations](FEATURES.md), [installation and recovery](INSTALLATION.md), and [release status](STATUS.md) for published-build details and verification limits.
