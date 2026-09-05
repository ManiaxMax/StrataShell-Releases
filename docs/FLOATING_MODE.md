# Tiled and Floating environments

Current Preview development and remaining acceptance are tracked in [release status](STATUS.md). Phase 5 makes Floating the fresh-release-install default only; existing profiles retain their selected environment. The installation and test counts below are historical evidence, not claims about the latest published package.

Installed locally with user approval on 2026-09-04 as release **20260904-101605**. The previous shell shut down normally through its rail; the existing bootstrap launched the new installed shell. Verified the active shell's executable path, installed/payload DLL hash match, schema 36 with background opacity 0.20, 92/92 installed self-tests, and the recovery shortcut. This establishes installed/running state; interactive acceptance of every behavior remains separate.

Follow-up fixes (source): minimized native caption strips are hidden while their iconic state and dock entries are preserved. Clicking a running dock icon always activates/restores it. STRATA title regions now include their blank space as a pointer target; dragging a tiled STRATA app releases it into the existing per-window floating presentation. Background dimming waits until opening reveals settle and releases its opacity before other presentation animations capture their baseline, preventing invisible app centers. The desktop signature moves to the top-left in Floating mode. Wallpaper, Desktop View and other OSD notifications appear at the top in Floating mode and at the bottom in Tiled mode, revealing inward from that edge. The default background transparency is now 80%; schema 36 migrates the old 90% default while preserving other customized values.

Follow-up verification: clean Release build; **84/84** private-desktop checks in `artifacts/desktop-mode-final/report.json`, including hidden minimized caption strips, dock activation, tiled title-drag release without snap-back, three animated app openings and mode-specific notification placement. The suite also checks 20 centered dock icons, workspace/monitor changes, native alpha, active-app overlap and uncover restoration, Files X/reopen, cancellation, and Windows-theme isolation. Native foreground ownership on the input desktop still requires interactive acceptance; the isolated test checks its own thread's native active HWND.

Press **Super + Shift + W**, or use **UI & Theme → Interface → Window Management Mode**. The choice persists across launches. Existing profiles remain Tiled after migration.

## Floating behavior

September 5 source follow-up: multiple document windows share one app icon with a window count. Hover for previews, click a group to choose a window, Shift-click to launch again, or right-click for Open new window, individual Close and Close all. Close sends the normal window message and respects unsaved-work cancellation. Live DWM thumbnails are paged and released on dismissal; minimized windows show a restore card. Windows minimize-target requests receive the visible icon's screen rectangle. STRATA minimize transitions use a bounded compositor thumbnail and preserve restore geometry. Fullscreen suppresses previews and transitions. See [current validation and limitations](STATUS.md).

The X closes STRATA Files, Settings, Snip and Keybindings instead of hiding a live app. Files can reopen as a fresh window, cancelled document closes remain usable, and pending Settings saves are retained.

First-party background dimming uses native whole-window alpha, preserving its layered style against WPF HwndTarget while dimming owns the window. The WPF content stays fully composed, including hosted content; activation and mode changes restore native opacity. The previous control-only fade left dark material rectangles.

Private-desktop and recovery self-test theme services cannot apply or restore Windows theme preferences. Disabling sync alone previously invoked the global backup restoration path and could change the active session to Light. Normal shell wallpaper/theme synchronization remains enabled according to the user setting.

Workspace switches retain the sideways swipe in both environments when Reduced Motion is off. Minimized windows are excluded from animation; maximized and normal floating windows retain their geometry. A dock selection remains the focus target after its workspace slides into view.

WPF workspace slides also commit their final state at the animation deadline. This prevents a switch to an empty workspace from hanging if offscreen windows stop receiving composition callbacks before the last frame.

Dismissed tray/quick panels stay hidden across wallpaper/settings changes and mode switches. Placement updates no longer use the native show-window flag. This fixes the empty 410×660 panel that could appear below the dock or beneath the top rail. Launcher placement follows the same visibility rule.

- Existing tiled applications become ordinary floating windows, sized within their monitor's work area. Previous floating geometry is reused when available.
- Drag title bars and resize edges without modifiers. Native and STRATA app minimize/maximize controls become available. Overlapping app windows stay visible; activating an app brings it forward.
- Inactive apps overlapped by the active app retain **20% of their configured opacity** by default (80% additional transparency). Uncovered inactive apps retain their normal opacity. Moving the active app away or activating the background app restores its captured opacity. Covered widget cards fade completely out and stop intercepting clicks; uncovered widgets remain visible.
- Click empty desktop space to minimize visible apps. Widget interactions are excluded. Restore using the dock or Alt + Tab; normal size/position and maximized state are retained.
- The complete icon collection stays centered on the monitor as applications open or close. Workspace-count changes and the active-monitor label do not shift its midpoint. A symmetric scrollable viewport keeps large collections clear of the side controls.
- The rail becomes a bottom dock with permanent STRATA launcher and default Browser/Files pins. STRATA icons use theme-bound vector artwork from the existing icon family. External apps retain their native icons. Running indicators, active highlights, grouped window menus, horizontal overflow scrolling, and persistent right-click pin/unpin are supported.
- Launcher, quick panels, tray, power, audio, and network surfaces anchor above the bottom bar. Their reveal direction follows the bar position; the tray arrow points up at the bottom and down at the top. The bar reserves native work area, and ordinary maximized windows stop at its inner border. Native borderless fullscreen and STRATA true fullscreen use the full display, suspend app transparency, and keep the active-window border, dock, and launcher suppressed even at monitor edges.
- Alt + Tab / Alt + Shift + Tab cycle apps on currently visible workspaces. Releasing Alt activates the selection; Escape cancels. Workspace switching, moving apps between workspaces/monitors, and recovery chords remain available. Tiling-only actions disappear from both routing and the keybinding list.
- Desktop View becomes Show Widgets / Hide Widgets per monitor. Existing monitor widget eligibility remains respected.

## Customization

Floating settings expose background transparency (0–98%), desktop-click minimization, and dock height (44–72 DIPs). Bar controls retain module visibility and edge/side spacing; tiled center-context controls are hidden. Changes apply live and persist independently from tiled layout preferences. Pins are managed directly from dock context menus.

## Returning to Tiled

The top rail, saved desktop views, controls preference, resize behavior and tiling shortcuts return. Floating dimming is removed. Managed apps are restored and distributed into two-app workspaces, adding workspaces as required up to the existing ten-workspace limit per monitor. If a monitor has over 20 managed apps, the mode switch asks that some be moved or closed first; it does not discard windows. Transient shell panels and owned modal dialogs retain their existing overlay roles.

## Verification

- Release build: zero warnings/errors.
- Quiet self-tests: **92/92 passed**.
- Private-desktop integration checks: **126/126 passed**, including real WPF/Win32 mode changes, bottom-dock maximize bounds, normal/maximized restore, overlapping visibility, background dimming, exact opacity-baseline restoration, live/persisted customization, dock pin lifecycle, Alt release, workspace moves, fullscreen return and retile capacity.
- Light/Dark dock captures were inspected. Evidence: `artifacts/desktop-mode-final/report.json`, `dock-dark.png`, `dock-light.png`.
- `scripts/Test-StrataDesktopMode.ps1` uses a private noninteractive desktop and isolated settings. It never switches the input desktop or reserves a native appbar. The internal test entry point refuses the interactive desktop.

## Installed acceptance still required

Coordinate installation/restart under AGENTS.md before interactive acceptance: physical keys and title/edge gestures; native app controls and transparency (including elevated apps); bottom appbar negotiation and maximized bounds on mixed-DPI monitors; empty-desktop clicks and widget hit testing; power/tray/native flyouts; repeated switching with the user's apps and pins. Source and isolated results do not certify the installed release.

Windows that require opaque composition retain the existing opacity compatibility protection. Apps whose executable/package identity cannot be determined are shown but cannot be pinned until a reliable launch identity is available. Browser-hosted pages share their browser identity; a separately installed ChatGPT app can have its own pin.
