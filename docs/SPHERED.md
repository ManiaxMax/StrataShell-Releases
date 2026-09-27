# Sphered window mode

Sphered is the third STRATA window mode, alongside Tiled and Floating. Choose it under UI & Theme → Interface → Window Management Mode, search for **Sphered mode**, or toggle it with **Super + Ctrl + S**. The former separate Sphere app launch now enters this mode.

Each monitor owns one Sphere space. Entering the mode collects open managed application windows on their respective displays. The live tray, network, Bluetooth, audio, battery, clock and system controls move into Sphere; the external rail and workspace numbers are absent. Leaving the mode undocks surviving applications. Closing an individual tab requests that application's ordinary close, including save prompts. Closing a Sphere leaves Sphered mode. Minimizing is disabled: there is no dock destination. Minimize controls and Show Desktop are unavailable, and app-initiated minimizing restores the app to its pane.

## Tabs and groups

- The centered main page contains search without shortcut boxes. Shell launchers, STRATA Command, setup and recovery utilities are excluded from app results.
- Search for apps, Settings, websites, web searches or explicit `>` commands. Enter opens the selected or first result; Up/Down changes the selection.
- The address bar, Back, Forward, Reload and GO controls appear only when the active tab contains a website or web search. Home, application, Settings and command tabs reclaim that row for content.
- Right-click a tab → **Group with…** and search the list for a partner, even if it is far away. Ctrl-click another ungrouped tab for a quick pair.
- Multiple independent pairs can coexist. Each pair shares an outline and displays two live panes. Ungroup retains both tabs; closing one expands its surviving partner.
- **Super + Ctrl + W** changes only the active monitor's selected group between side-by-side and top/bottom.
- **Super + Shift + Arrow** moves the active pane within its pair along that orientation. For an ungrouped tab it reorders the tab strip.
- **Super + Alt + Shift + monitor number** moves the active tab. Moved applications undock onto their destination monitor. The right-click **Move to monitor…** submenu has been removed; use Ctrl-drag or the shortcut.
- Hold **Ctrl** and left-drag a tab to reorder it or drop it into another Sphere. Drop on the left/right half of a tab to insert before/after it; empty space appends it. Reordering a grouped tab moves its pair together; dragging to another Sphere moves the individual tab. Ctrl-click without dragging still groups tabs.
- Launching a single-instance app that reactivates its existing window pulls that tab into the requesting Sphere, retaining the app's state. Steam's existing client window is also recognized when Windows does not deliver foreground activation. New windows take priority, and first-party multi-instance apps such as STRATA Files continue opening independent windows.
- **Super + Tab**, **Super + Right** and **Super + Left** select the next/previous tab; **Super + +** opens a new main tab. The list and editor show these contextual actions in Sphered and restore desktop labels on exit. Remaps retain their original shortcut identities and conflict checks.
- **Super + T** toggles transparency for the active tab only. Games start opaque; ordinary apps and websites start transparent. Game detection uses current Steam install manifests and standalone Unity/Unreal engine markers. Unknown standalone engines may need the manual toggle. Tabs retain their choice when switched, moved between monitors or reconnected to a replacement native window.
- **Super + \** is disabled in Sphered. Native maximization/fullscreen requests are returned to the pane; website fullscreen requests are exited.
- **Alt + Tab / Alt + Shift + Tab** cycles tabs in the active Sphere. **Super + Q** closes the active tab. Ctrl+T/W/L/Tab works when Sphere receives keyboard input; native applications keep their own shortcuts.

## Native application behavior

Native applications retain top-level Windows handles and their own input/DPI contexts. Sphere logically owns their geometry and visibility; they are excluded from the outer desktop layout and dock. This replaces cross-process child-window parenting. Window chrome is captured before WPF changes and restored when undocked. Native placement finishes before desktop ownership resumes; pending tab placement stops at the start of undocking. Inactive tabs preserve their visible-window rendering lifecycle using DWM cloaking when available and off-screen parking otherwise; returning commits on-screen geometry and requests client/child repaint. Launch resolution supports process handoffs, reused process windows and packaged application frames. Steam launcher ancestry includes its browser helper process. A game that replaces its native window during startup can reconnect to its tab.

Monitor transfers retain the same native host and continuously registered ownership. Pending native placement drains before the host changes Sphere, preserving scale/transparency/restoration state and moving launcher-child observation to the destination. A transfer no longer restores the app to the desktop between spaces.

New game windows follow the launcher's current Sphere rather than their initial Windows monitor. The native show observer and periodic desktop collector use the same routing decision; the closest watched process ancestor wins when a launcher and its game occupy different spaces. Already-open windows retain their monitor when entering Sphered. Steam URL/search launches also retain the requesting Sphere for up to three minutes, using installed manifests and library folders discovered at launch time. This includes newly installed games and libraries on other drives, without fixed game IDs or machine paths. A successful adoption consumes that request, so later child windows follow the tab's current space. Routing does not change a game's internal rendering mode: automatic borderless conversion, including Quake Champions, remains unresolved.

Apps resize directly to their pane. **Fit app to pane** provides an aspect-preserving DWM scaled presentation for minimum-size applications. **Resize app directly** returns to native input and resizing. Scaled presentation forwards basic pointer and keyboard messages; raw input, IME, elevated/protected windows and exclusive fullscreen still require application-specific acceptance. Windowed/borderless rendering is the compatible game path. Secure Windows surfaces remain Windows-owned.

The tab menu offers a separately confirmed **Force close app** for a stopped external application. This terminates only the window's process, never its descendant game process tree. All windows sharing that process may close and unsaved changes can be lost. Shared Windows hosts and the shell cannot be force-terminated through a tab. If undocking stalls, Sphere retains the tab and its recovery controls rather than discarding it. Shell-mode docking records native styles and bounds in the existing recovery journal so bootstrap recovery can restore them after a shell failure.

Websites and searches use STRATA Browser with its normal profile, navigation, permissions and download handling. Browser title, internal tabs, menu and status chrome are hidden inside Sphere. Page titles appear in Sphere tabs. Existing Browser tabs split into separate live Sphere pages without reloading; leaving Sphere restores each page as an independent standard Browser window on its monitor. Browser event ownership transfers with each live page.

Sphere display surfaces bypass ordinary application size limits and fullscreen suppression. Each status control opens its panel on the monitor that supplied the click, and mode changes show the Sphered notification.

Sphere's display bounds are enforced before native move/resize requests are applied. Header dragging, double-clicking, system maximize/restore/minimize commands and programmatic state changes cannot turn a display space into a floating window. Tabs share the top status rail; the removed tab row and bottom status bar are reclaimed by app/page content. Saved Sphered sessions suppress desktop widgets, edges and separate rails before the first show; each Sphere is positioned on its own display before opening.

Tab restoration waits for the selected pane's layout, retains its raise request until the pane is visible and returns native focus when switching from the same Sphere. An inactive native foreground window cannot reverse the selected tab. First-party apps created inside Sphere restore their taskbar state and register desktop movement/chrome on undocking; already registered apps reapply the destination mode's chrome.

## Compatibility and release status

Native application hosting is best effort. Protected or elevated windows, raw-input games, IME, and exclusive fullscreen may require leaving Sphered mode. Windowed or borderless games are the compatible path; closing a tab uses the application's normal close behavior, including save prompts.

Desktop widgets on new-tab pages use each monitor's current assignments and settings. Super + Ctrl + W toggles widgets on the active monitor's new-tab page; on an app tab it changes the grouped split direction.

See [release status](STATUS.md) for the published builds and verification limits.
