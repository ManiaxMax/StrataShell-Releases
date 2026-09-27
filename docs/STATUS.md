# Release and testing status

Updated September 27, 2026. [Stable 1.0.23](https://github.com/ManiaxMax/StrataShell-Releases/releases/tag/v1.0.23) and [Preview 20260927-182410](https://github.com/ManiaxMax/StrataShell-Releases/releases/tag/20260927-182410) are the current published packages. The private source repository has no GitHub Releases.

Stable 1.0.23 offers self-contained Windows 11 x64 Setup and portable packages with the .NET 10 Desktop runtime and exactly 24 approved 4K glass wallpapers. Preview is a lightweight update without a bundled runtime or wallpaper library; it needs a compatible Stable installation or registered .NET 10 Desktop runtime. See [the 1.0.23 notes](RELEASE_NOTES_1.0.23.md) and the selected Preview release for exact contents.

The 1.0.23 publication record reports four warning-free Release builds, 280/280 quiet checks, 36/36 wallpaper packaging checks, 46/46 safe-default checks, 6/6 installer-profile checks, 30/30 release-safety checks, and 9/9 runtime-lifecycle checks. An isolated installer test passed without changing the production shell or Explorer. These are source and package checks, not physical installed-shell acceptance. Publication did not install, activate, or restart STRATA on the owner machine.

Fresh installations use [shipped defaults](SHIPPED_DEFAULTS.md); updates and repairs preserve existing preferences. [Features and limitations](FEATURES.md) identifies hardware and Windows-owned boundaries. Downloaded executables do not yet have Windows Authenticode publisher signatures. [Installation and recovery](INSTALLATION.md) describes the bootstrap watchdog, emergency Explorer chord, and external recovery shortcut.

Developer evidence stays in the private source repository. A documentation update does not create a new application package or alter an installed shell.
