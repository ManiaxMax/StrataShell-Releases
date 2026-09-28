# Installation, updates and recovery

STRATA Shell can run as a preview without changing Windows sign-in, or as the current account's desktop shell. Read the recovery steps below before activating it as your sign-in shell. See [release status](STATUS.md) for the packages currently published.

## Choose a package

- **Stable 1.0.23** provides Windows 11 x64 Setup and a portable ZIP. Both include the .NET 10 Desktop runtime and 24 approved 4K wallpapers. It is the route for a fresh installation.
- **Preview 20260927-182410** is a smaller update ZIP. It omits Setup, the runtime and wallpapers. It needs a compatible Stable installation or a registered .NET 10 Desktop runtime and preserves the installed wallpaper library.

Use the downloads on the [Stable release page](https://github.com/ManiaxMax/StrataShell-Releases/releases/tag/v1.0.23) or select a channel in **Settings → Updates**. Downloaded executables do not yet have Windows Authenticode publisher signatures, so Windows may show Unknown Publisher. STRATA update archives use a separate signed manifest and checksums.

## What installation changes

The installer verifies its payload and runs STRATA's noninteractive self-test before changing the configured shell. It installs an immutable local release, preserves an existing user profile during updates or repairs, and creates recovery tools outside the application folder. A new installation starts with the [documented defaults](../README.md#fresh-install-defaults).

Windows 11 Home and Pro use a reversible current-user custom-shell policy. Supported Enterprise, Education and IoT Enterprise editions use Windows Shell Launcher for the current account; Explorer remains its fallback. STRATA does not replace Windows sign-in, UAC, the secure desktop, drivers or Windows Recovery.

STRATA Settings → Updates checks the public release feed without a GitHub account. **Check for updates** does not download a package. **Update now** downloads, verifies, self-tests and installs the chosen release. The running shell is not overwritten; the new release is used at the next sign-in. If validation fails, activation stops and the previous installation remains available.

## Restore Explorer

**Explorer Session** in the Power panel is temporary for the current login. STRATA returns at the next sign-in if it is still configured as the shell.

To make Explorer the default shell permanently, use **Settings → Windows Tweaks → Default Shell** and turn off **Make STRATA Shell My Default Shell**. The action asks for confirmation and uses the Windows-edition-appropriate recovery route.

If STRATA is unresponsive, press **Ctrl + Alt + Shift + Delete**. The external recovery shortcut is also available at:

```text
%USERPROFILE%\Strata Recovery\Return-To-Explorer.cmd
```

You can open Task Manager with **Ctrl + Shift + Esc**, choose **Run new task**, and run that command. Keep the recovery shortcut available outside STRATA.

A bootstrap watchdog starts a temporary Explorer session after three unexpected STRATA exits within ten minutes. It can also offer a confirmed temporary Explorer session when the shell stops responding. It does not automatically close a slow application. Recovery and startup records live under `%LOCALAPPDATA%\StrataShell\Recovery` and may contain personal paths or device information; do not post them publicly without review.

## Limits

An automated build, package self-test or isolated installer check does not prove physical input, third-party app compatibility, monitor behavior or an installed desktop session. See [features and limits](FEATURES.md) and [current verification status](STATUS.md).