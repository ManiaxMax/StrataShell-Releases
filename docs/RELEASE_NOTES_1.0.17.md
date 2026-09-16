# STRATA Shell 1.0.17

Stable promotion of Preview 20260908-224306, including the accumulated desktop and application improvements since 1.0.16.

- Low/High bloom quality and selectable bloom scope. High performance preset now uses economical bloom with frosting and disabled window/widget shadows.
- Improved floating snap previews, minimized-window previews, fullscreen restoration, workspace motion controls, and mode-switch responsiveness.
- Windows theme-change notifications run off the shell UI thread to avoid waits on other applications. Wallpaper changes no longer display the filename banner.
- Polished confirmations and notifications, two-tone application icons, STRATA Notepad naming and toolbar typography, and handwritten Notes text.
- Improved Paint/Snip color controls, USB ejection capability detection, Wi-Fi automatic connection choices, and wallpaper transition options.
- Application shortcut overrides preserve protected system bindings. Workspace changes and moved windows report their destination.
- Manual clock changes explicitly disable network synchronization; automatic time can be restored in settings.

## Package and validation

Includes a self-contained Windows x64 installer and portable/update bundle with the .NET 10 runtime. Existing user profiles are preserved; fresh installs use the approved safe defaults.

Release build and self-tests passed during Preview validation. Desktop acceptance passed 583 checks; focused tests covered bloom, dialogs, preset matching, clock sequencing and asynchronous theme notifications. A separate live Floating keybinding-map assertion remains unresolved. Installed theme-switch and manual-clock reboot verification remain pending; no automatic installation or shell activation is performed by publication.
