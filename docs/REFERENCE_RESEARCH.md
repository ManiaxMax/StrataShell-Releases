# Reference research

Research snapshot: 2026-08-25. Strata borrows principles, not protected assets, naming, layouts, or code.

## Evidence matrix

| Finding | Evidence | Likely cause | Strata response | Validation |
|---|---|---|---|---|
| Hyprland feels immediate because layout, focus, workspace, opacity, border, blur, and animation are one coherent system. | [Hyprland window rules](https://wiki.hypr.land/Configuring/Window-Rules/) expose those states as compositor concepts. | A single authority owns placement and feedback. | One window registry and one layout transaction feed borders, animation, workspace state, and the bar. | Rapidly open/close/swap ten windows; no stale borders or layout jumps. |
| Caelestia feels unified because panels morph out of a connected edge shell. | [Caelestia shell](https://github.com/caelestia-dots/shell) describes a fluid, morphing Quickshell shell. | Shared geometry, origin-aware motion, and one popout coordinator. | Continuous perimeter, connected corners, edge-relative popouts, one active primary popout per monitor. | Record every popout at 60 FPS; origin and dismissal must remain spatially consistent. |
| Quickshell separates edge-reserving panels from anchored popups and provides desktop-service integrations as shell primitives. | [PanelWindow](https://quickshell.org/docs/v0.2.1/types/Quickshell/PanelWindow/), [PopupWindow](https://quickshell.org/docs/v0.2.1/types/Quickshell/PopupWindow/), and [Quickshell overview](https://quickshell.org/about/). | Shell surfaces need different focus, z-order, anchoring, and lifetime rules than app windows. | Distinct desktop, frame, rail, popout, OSD, and app-window types with a shared coordinator. | Focus, z-order, exclusive-area, multi-monitor, and light-dismiss tests. |
| TMOG achieves a technical look through restrained phosphor color, dense native information, hairlines, dark depth, and adjustable saturation—not through generic neon decoration. | [TMOG](https://www.tmog.org/) presents live dense instrumentation and light/dark/green/amber/blue/mono treatments with saturation control. | Signal color is scarce and tied to meaning. | Wallpaper-derived phosphor tokens, monochrome structure, thin geometry, optional bloom and vibrancy. | Grayscale and high-vibrancy screenshots must retain hierarchy and text contrast. |
| macOS panels are temporary, subordinate, context-aware surfaces; hierarchy is kept shallow and content can extend beneath translucent navigation. | Apple’s [panels](https://developer.apple.com/design/human-interface-guidelines/panels), [sidebars](https://developer.apple.com/design/human-interface-guidelines/sidebars), and [menus](https://developer.apple.com/design/human-interface-guidelines/menus) guidance. | Predictable material and behavior matter more than ornament. | Popouts hide when context ends, menus stay shallow, first-party tools use clear split hierarchies. | Keyboard focus returns correctly and no transient surface appears in app/window lists. |

## Preserve, adapt, avoid

### Preserve principle

- Hyprland: coherent window state, fast directional control, and visible active state. Strata deliberately rejects free floating in favor of its fixed center stage.
- Stable mnemonic `Super` grammar and discoverability.
- riced Linux: personal expression, wallpaper-led palettes, compact status language.

### Adapt

- Wayland layer-shell becomes Win32 appbar/z-order and monitor-aware HWND policy.
- Hyprland compositor transactions become coordinated `SetWindowPos` batches.
- Linux audio/network/Bluetooth providers become Core Audio, Native Wi-Fi, WinRT device, and monitor APIs.
- QML hot reload becomes a versioned JSON theme/widget contract with reloadable first-party modules.

### Avoid

- copying Caelestia geometry, icons, source, or themes verbatim
- pretending DWM exposes Wayland-compositor powers it does not
- excessive translucent layers, unreadable accent text, or bloom everywhere
- card grids that turn every panel into a dashboard
- a Windows taskbar/Start-menu silhouette with different colors
