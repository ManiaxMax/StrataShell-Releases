# Safe shell mode

This page is retained for old links. The current installer detects the Windows edition: Home/Pro use a reversible current-user custom-shell policy; supported Enterprise/Education/IoT editions use **Windows Shell Launcher**, assign Strata only to the current user SID, and retain `explorer.exe` as the default fallback. Both paths start a three-failure bootstrap watchdog.

Do not directly edit Winlogon or copy old registry snippets. Use the edition-aware installer so the original value, validation marker, immutable release, and Explorer recovery route are created together.

Use the complete current guide:

- [Installation and recovery](INSTALLATION.md)
- [Architecture: recovery](ARCHITECTURE.md#recovery-architecture)

The emergency controls are:

- `Ctrl + Alt + Shift + Delete` — exit preview or restore Explorer in installed shell mode.
- `%USERPROFILE%\Strata Recovery\Return-To-Explorer.cmd` — external permanent recovery route.

ChatGPT / Codex remains available through `Super + C` and its entry in Strata Settings; the retired alternate “lifeline” chord is not part of the current binding policy.
