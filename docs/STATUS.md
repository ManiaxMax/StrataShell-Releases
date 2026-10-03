# Release and testing status

Updated October 3, 2026 for [Stable 1.0.24](https://github.com/ManiaxMax/StrataShell-Releases/releases/tag/v1.0.24) and [Preview 20261003-095029](https://github.com/ManiaxMax/StrataShell-Releases/releases/tag/20261003-095029). The release feed confirms package availability. The private source repository has no GitHub Releases.

Stable 1.0.24 offers self-contained Windows 11 x64 Setup and portable packages with the .NET 10 Desktop runtime and exactly 24 approved 4K glass wallpapers. Preview is a lightweight update without a bundled runtime or wallpaper library; it needs a compatible Stable installation or registered .NET 10 Desktop runtime. See [the 1.0.24 notes](RELEASE_NOTES_1.0.24.md) and the selected Preview release for exact contents.

The 1.0.24 source passed five warning-free Release builds, 296/296 quiet checks, localization coverage for 25 catalogs, keyboard routing, release-safety checks and safe-default checks. Its preceding Preview passed 103/103 focused dock/panel checks. The isolated installer lifecycle test passed with `ProductionShellOrExplorerChanged: false`. These are source and isolated checks, not physical installed-shell acceptance. Publication does not install, activate, or restart STRATA on the owner machine.

Fresh installations use the [default layout](../README.md#fresh-install-defaults); updates and repairs preserve existing preferences. [Features and limitations](FEATURES.md) identifies hardware and Windows-owned boundaries. Downloaded executables do not yet have Windows Authenticode publisher signatures. [Installation and recovery](INSTALLATION.md) describes the bootstrap watchdog, emergency Explorer chord, and external recovery shortcut.

Developer evidence stays in the private source repository. A documentation update does not create a new application package or alter an installed shell.
