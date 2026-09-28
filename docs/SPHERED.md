# Sphered mode

Sphered is the third window mode alongside Floating and Tiled. Select it in **Settings → UI & Theme → Window Layout** or press **Super + Ctrl + S**. Each monitor gets its own Sphere with tabs for applications, websites, searches and Settings. Leaving the mode returns surviving apps to the desktop. The usual outer rail, dock and workspace strip are replaced by Sphere's own status controls.

## Tabs and groups

- Open an app, website or web search from the main page. Websites show navigation controls; app tabs use the space for content.
- Right-click a tab and choose **Group with…**, or Ctrl-click another ungrouped tab, to make a two-pane group. Multiple groups can coexist. **Super + Ctrl + W** switches the active pair between side-by-side and top/bottom.
- **Super + Tab** selects the next tab; **Super + Shift + Tab** selects the previous one. **Super + +** opens a new main tab. **Alt + Tab** also cycles tabs while Sphere is active.
- **Super + Shift + Arrow** moves the active pane within its pair or reorders an ungrouped tab. Ctrl-drag a tab to reorder it or move it to another Sphere. **Super + Alt + Shift + monitor number** moves the active tab to a monitor.
- **Super + T** changes transparency for the active tab. Games start opaque, but an unrecognized game may need a manual change.
- **Super + Q** requests the active app's normal close, including save prompts. Closing the Sphere exits Sphered mode.

New-tab pages can show the active monitor's widgets. On a new-tab page, **Super + Ctrl + W** toggles those widgets; on an app tab it changes the selected group's split direction. See the [keybindings guide](KEYBINDS.md) for mode-specific shortcuts.

## Native apps and limits

STRATA keeps compatible native apps running with their own input and rendering while presenting them in a pane. Some applications with hard minimum sizes can use **Fit app to pane**; **Resize app directly** returns to normal input and sizing. Scaling can limit raw input, IME or protected/elevated content. Windowed or borderless games are the compatible path; exclusive fullscreen and secure Windows surfaces remain Windows-owned.

A launcher may open a game in a new window, and STRATA attempts to keep that window with the requesting Sphere. This is best effort for Steam, game engines and single-instance apps. Games that replace their window or control their own display mode can require leaving Sphere. See [release status](STATUS.md) for the published builds and installed acceptance limits.