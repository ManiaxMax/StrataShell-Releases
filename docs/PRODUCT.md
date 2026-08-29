# Product contract

This is the design contract for StrataShell. For the exact behavior implemented today, use [Features and limitations](FEATURES.md).

## North star

Strata should feel like a purpose-built technical environment: calm at rest, fast under the keyboard, spatially coherent, and vividly alive when it moves. It must not resemble a reskinned Windows taskbar, Start menu, Explorer, or Settings app.

Windows remains underneath for application compatibility, drivers, security boundaries, and hardware APIs. The user-facing desktop belongs to Strata.

## Product principles

1. **Center Stage, not window clutter.** Applications occupy one predictable center lane. A workspace has one full-height app or two top/bottom apps—never a pile.
2. **Wallpaper is live state.** Every wallpaper change can change palette, light/dark mode, focus color, glass, widgets, consoles, and compatible application preferences.
3. **Keyboard first, pointer complete.** Frequent actions have a stable `Super` chord, while every visible shell control remains clickable.
4. **Spatial continuity.** Workspace changes move laterally, top/bottom exchanges move vertically, and edge controls expand from their origin.
5. **Persistent peripheral awareness.** Widgets remain around the center lane and yield when content needs their space.
6. **Signal over decoration.** Color, bloom, motion, and translucency convey state. They must not erase hierarchy or legibility.
7. **Recovery is a feature.** A shell that can strand the user is not complete.

## Design language

- **Continuous surround:** a thin, wallpaper-colored perimeter turns the display into one visual instrument.
- **Phosphor glass:** translucent dark or mineral-light surfaces, precise hairlines, compact status type, adjustable vibrancy, and restrained CRT bloom.
- **Riced composition:** wallpaper, palette, widgets, gaps, rail, and Center Stage behave as one composition rather than separate skins.
- **Technical density:** useful live information is welcome; visual hierarchy comes from alignment, rhythm, and contrast rather than nested cards.
- **Opposite of Windows:** familiar security and compatibility remain underneath, but the visible navigation model is Strata's own.

## Interaction contract

### Center Stage

- One application uses the complete rendered widget-height envelope.
- Two applications use equal-width top and bottom slots.
- A third application flows to the next workspace with capacity and becomes visible there.
- Slot movement into an occupied destination performs an exchange.
- Fullscreen is temporary, opaque, and reversible to the exact centered slot.
- Widgets remain outside the application lane and persist across workspaces.

### Workspaces

- Directional input must preserve the user's layer: top stays top and bottom stays bottom when traversing workspaces.
- Motion must complete the full distance before the old state is removed.
- Focus, active border, rail state, and application geometry must update as one transaction.

### Transient surfaces

- Only one primary quick panel is active at a time.
- Launchers, keybind search, Settings, Files, and prompts receive usable keyboard focus immediately.
- Widgets fade or contract when another surface needs the same pixels.
- Dismissal restores meaningful focus instead of sending input to the desktop.

## Appearance contract

The theme engine has Dark, Light, and Auto modes. Auto evaluates wallpaper luminance. Palette extraction produces accent, secondary, success, warning, surface, text, muted, and border roles, with contrast guards that retain palette identity.

User controls remain independent:

- vibrancy;
- glass opacity;
- blur strength;
- bloom strength;
- surround intensity and thickness;
- active-border thickness;
- animation speed;
- unified UI scale;
- reduced motion; and
- high contrast.

First-party surfaces must update live. External adapters are opt-in and reversible.

## Performance intent

These are engineering targets, not certified current benchmarks:

- quiet event-driven idle behavior rather than high-frequency polling;
- responsive launcher and workspace input;
- 60 Hz animation as a baseline and high-refresh timing where DWM/display support it;
- no avoidable decode or network work on the animation path;
- reduced-effects mode that preserves layout and meaning; and
- first-party widgets that pause or reduce expensive work when hidden.

Measured benchmark results should be published only with hardware, display mode, build, and sampling method.

## Safety contract

- Preview is the default development mode.
- The installed executable must self-test before shell activation.
- Explorer remains Shell Launcher's default fallback.
- Recovery tools live outside the versioned application folder.
- At least one ChatGPT/terminal path and the Explorer recovery path do not depend on Explorer.
- Repeated early crashes restore Explorer rather than forming a login loop.
- Strata never replaces UAC, sign-in, `Ctrl + Alt + Delete`, or Windows Recovery.

## Release gates

A release candidate is not called stable until it has evidence for:

- clean preview and installed-shell startup;
- reversible Explorer recovery from the UI, emergency chord, and external script;
- complete one/two-app, overflow, exchange, focus, workspace, fullscreen, and close/reflow flows;
- Win32 and packaged-app discovery and launch;
- Files operations on local, redirected, network, and removable locations;
- wallpaper/theme propagation in Dark, Light, and Auto modes;
- keyboard and pointer operation, reduced motion, high contrast, multi-DPI, and representative accessibility checks;
- representative audio, network, Bluetooth, brightness, and power/session behavior;
- measured idle/animation performance;
- signed, versioned, uninstallable distribution; and
- documented known limitations and rollback.

The current repository is experimental alpha and does not claim that all stable-release gates have passed.
