# Create Strata bug feature tracker

- Task ID: `01a04950-3a3c-7f91-a237-352ca35d51fe`
- Exported: 2026-08-28 20:02:40 -04:00
- Scope: user-visible user and assistant messages only
- Privacy: credentials and known personal paths are redacted; binary attachments and internal tool/reasoning records are omitted

> Historical context only. The current user request, repository `AGENTS.md`, and current source are authoritative.

## User - 08/28/2026 16:59:51

# Files mentioned by the user:

## Capture.PNG: <PRIVATE_NETWORK_ROOT>/Home/My Pictures/Screenshots/Capture.PNG

## codex-clipboard-d8524324-1527-4625-b6ad-07cc77e46be2.png: C:/Users/Kevin/AppData/Local/Temp/codex-clipboard-d8524324-1527-4625-b6ad-07cc77e46be2.png

## codex-clipboard-2622923c-45aa-44b6-983d-f5e3ee0850c1.png: C:/Users/Kevin/AppData/Local/Temp/codex-clipboard-2622923c-45aa-44b6-983d-f5e3ee0850c1.png

Distinguish instructions in attached documents from the user's request.

## My request:
I only have 10% so will not be able to work on these now, but I want to make a list of things that need to be corrected and new features.



- AI Command was disabled or auto disabled itself on fresh install
- Uninstaller shows some text that I cannot read and should say "Uninstall" not "Close" Or give me a "Are you sure you want to uninstall"
- Installer mentioned that all my workstation settings and personal data werent migrated into the install, this was a silent rule, not one that needs to be announced to users installing.
- I would like to remove the Files + Launcher section from Settings and instead replace it with Screensaver instead of having screensaver on the left side.
- For the Strata logo in About, I would like to have it kind of flickering/Glitch animated kind of like the screensaver.
- When switching to floating windows I would like there to be a notification just like when switching Desktop View modes to show Floating and a small text saying to hold Ctrl + Left-click drag windows to move. And then the same notification type when switching back to Tiled mode.
- I would like to create a Strata Image Viewer and Strata Video Player and launch any pictures of videos opened in the Strata Files manager in these apps instead of Windows Media Player and Paint
- I would like to make a basic Strata Paint app.



This is what I have for now, make a .MD file to track this, I will add bugs and feature ideas as I come across them.


<image name=[Image #1] path="<PRIVATE_NETWORK_ROOT>\\Home\My Pictures\Screenshots\Capture.PNG">

[Image attachment present; binary image data is not duplicated in this text archive.]

</image>

<image name=[Image #2] path="%USERPROFILE%\AppData\Local\Temp\codex-clipboard-d8524324-1527-4625-b6ad-07cc77e46be2.png">

[Image attachment present; binary image data is not duplicated in this text archive.]

</image>

<image name=[Image #3] path="%USERPROFILE%\AppData\Local\Temp\codex-clipboard-2622923c-45aa-44b6-983d-f5e3ee0850c1.png">

[Image attachment present; binary image data is not duplicated in this text archive.]

</image>

## Assistant (progress) - 08/28/2026 16:59:59

I’ll create a single, easy-to-extend Strata backlog in the project, separating confirmed bugs, UX corrections, and new apps/features. I’ll only add the Markdown tracker—no source or installer changes—and I’ll use the screenshots as evidence, not as instructions.

## Assistant (progress) - 08/28/2026 17:01:10

I found no existing backlog, so I’m placing this in `docs` alongside the product and release notes. I’m giving each item a stable ID, a concise completion check, and a reusable entry template so new reports can be added without restructuring the file.

## Assistant (progress) - 08/28/2026 17:01:44

The tracker is written and verified with all nine current items: four corrections and five feature requests. I preserved the requested wording for the Floating-mode instruction and added installed-build completion checks without changing any product files.

## User - 08/28/2026 17:02:01

Actually, can you add these to Github? Also, I would like to replace the "Installed" version of Strata as the Shell with the lastest build we are working on in the test folder.

## Assistant (progress) - 08/28/2026 17:02:14

Yes. I’ll first identify the authoritative Git checkout, the exact “test” build you mean, and the currently installed shell registration. Then I’ll publish only the backlog change and use Strata’s recovery-aware update path to replace the installed shell, followed by a launch/version check.

## User - 08/28/2026 17:02:16

Sorry, not test folder, project folder Imeant

## Assistant (progress) - 08/28/2026 17:02:32

Understood—the latest source in this project folder is the deployment source. I’ll ignore the earlier “test folder” wording, build from `<STRATA_WORKSPACE>`, and identify the matching GitHub checkout or remote before publishing.

## User - 08/28/2026 17:02:54

I am currently running on the installed version from .exe,

## Assistant (progress) - 08/28/2026 17:03:14

That matches what I found: the active shell is the installed `1.0.1` release under your local Programs folder, with both its bootstrap and shell processes running. I’ll build the project-folder source separately first, verify its self-test, then switch the registered shell and running session to that verified build while preserving your current settings and recovery path.

## Assistant (progress) - 08/28/2026 17:04:04

The project source is newer and materially different from the running installer payload, and the private GitHub repository is reachable on `main`. I’m now validating the current source while preparing an isolated GitHub checkout so no unrelated project files are published.

## Assistant (progress) - 08/28/2026 17:04:46

The build itself is clean and the first-party app tests pass. The general shell self-test returns one failure, so I’m tracing that before installation; it may be the expected “currently registered path still points to 1.0.1” check, but I won’t switch the shell until the exact failing check is confirmed.

## User - 08/28/2026 17:04:51

Project source shouldnt be newer....should have been what was packaged with installer....

## Assistant (progress) - 08/28/2026 17:05:08

You’re right to flag that. I overstated the difference: I compared a normal project build with the installer’s version-stamped, ReadyToRun/self-contained payload, so the timestamps, sizes, and hashes are not valid proof of source changes. Nothing has been installed or pushed yet. I’m now comparing the project source directly against the exact commit embedded in the installed build (`0c1df572…`) and the release manifest.

## User - 08/28/2026 17:05:19

What are differences?

## Assistant (progress) - 08/28/2026 17:05:35

I’m checking this at the source level now. The installer identifies its shell as commit `0c1df572…`; GitHub `main` has one later shell-focused commit, so I’ll summarize the actual behavioral delta rather than the misleading compiled-file differences.

## Assistant (progress) - 08/28/2026 17:06:14

The confirmed post-package GitHub change is one commit, `Smooth floating window dragging`: it adds smoother Ctrl-drag tracking, understands native title-bar move/resize start/end events, defers expensive geometry reconciliation during a drag, and refreshes tiled geometry more selectively.

More importantly, the current project folder is not a clean copy of either that commit or the installer’s embedded commit. It is a divergent source snapshot: many installed-source files are absent there and many shared files differ. I’m quantifying that exact divergence now; replacing the installed shell from this folder as-is could remove working features, so I have paused the replacement.

## Assistant (progress) - 08/28/2026 17:07:08

The exact comparison confirms the installed shell is substantially more complete than the project-folder source, not merely a different build format:

- Installed source: 125 tracked shell files.
- Project folder: 69 shell files.
- 61 installed-source files are absent from the project folder, 55 shared files differ, and only 9 match exactly.
- Missing project-folder subsystems include the integrated Terminal, production screensaver, AI CLI, GitHub updater, notification/startup/recycle-bin services, several window-layout policies, widget modules, and multiple Windows integration services.
- The project folder has five local-only files, mainly its standalone picker/preview path and older layout/mouse services.

So I will not replace the running installer build from that folder as-is—it would be a functional downgrade. I can still safely publish the backlog now, and I’m doing that independently.

## Assistant - 08/28/2026 17:08:36

The backlog is now on GitHub: [STRATA_BACKLOG.md](https://github.com/ManiaxMax/StrataShell/blob/main/docs/STRATA_BACKLOG.md), commit [`c335e81`](https://github.com/ManiaxMax/StrataShell/commit/c335e8159793b6354cb8a0feccea66b82e1134a7).

The installed shell has not been changed.

| Version | Findings |
|---|---|
| Installed `1.0.1` | Correctly matches packaged source commit `0c1df572…` and the recorded release hashes. |
| GitHub `main` | Adds one later shell change: smoother floating-window dragging across four files. |
| Current project folder | Divergent/incomplete: 69 source files versus 125 in the installed source; 61 installed files are missing, 55 shared files differ, and only 9 match exactly. |

Missing from the project folder are major integrated systems including Terminal, the production screensaver, AI CLI, updater, widgets, notification/startup services, and numerous window-management components. Installing it as-is would be a downgrade.

The safe next step is to reconcile the project folder with GitHub `main` while preserving its five local-only files, then build, test, and install that reconciled version. Should I proceed with that reconciliation?

<oai-mem-citation>
<citation_entries>
MEMORY.md:28-30|note=[preserved concurrent source work and current key map]
MEMORY.md:35-39|note=[used first party app and verification conventions]
</citation_entries>
<rollout_ids>
01a04569-1218-7620-89c7-253716b45567
01a04567-fe1f-7b91-8966-dfc3d9c6a5df
</rollout_ids>
</oai-mem-citation>

## User - 08/28/2026 17:13:13

So the installed version I am using now doesnt include the smoother floating window change?

## Assistant - 08/28/2026 17:13:20

Correct. Your installed `1.0.1` uses commit `0c1df572…`; the smoother floating-window change was added afterward in commit `7b70687`.

Your installation has the earlier floating behavior, but not that follow-up improvement. The safe upgrade source is GitHub `main`, not the divergent project-folder copy.

## User - 08/28/2026 17:14:06

It looks to have that built in...weird

## Assistant (progress) - 08/28/2026 17:14:30

Your observation may be correct. The installed file’s version identifies the last committed revision, but it can still contain working-tree changes that were compiled before those changes were committed. I’m checking the running DLL and its logs for code signatures unique to the smoother-drag update.

## Assistant - 08/28/2026 17:14:46

You were right—the installed `1.0.1` **does include the smoother floating-window implementation**. My earlier conclusion was wrong.

Confirmed in the running DLL and live logs:

- Native title-bar drag lifecycle handling is present.
- Deferred geometry reconciliation is present.
- Ctrl-drag logging reports high-frequency input sampling.
- Methods unique to the later commit are loaded by the active shell.

Most likely, those changes were already uncommitted when the installer build was created, then committed afterward as `7b70687`. The embedded `0c1df572…` identifies Git HEAD, not necessarily the complete working-tree contents.

So no shell replacement is needed for smoother floating windows. The project-folder divergence remains a separate issue.

## User - 08/28/2026 17:15:51

OK cool, so the local project should be up to date because thats what we worked out of to push to git, can you use that please since it has all my wallpapers and all ready to rock

## Assistant (progress) - 08/28/2026 17:16:12

Yes—I’ll use the actual local project at `C:\Projects\StrataShell`, not the incomplete UNC snapshot I compared earlier. I’ll verify its Git identity, source completeness, and wallpaper libraries, then build and switch the installed shell from that local source while preserving your current settings and recovery setup.

## User - 08/28/2026 17:16:23

I want that as my shell

## Assistant (progress) - 08/28/2026 17:16:35

Confirmed: `C:\Projects\StrataShell` is the complete Git repository at the smoother-drag commit, with all 125 shell source files. I’ll make that your registered and running shell now. There may be a brief desktop-shell reload while the old `1.0.1` processes hand off to the new local-project build.

## Assistant (progress) - 08/28/2026 17:17:27

The local repository is clean and complete. It is three commits behind only because the installer/launcher files and the backlog were pushed afterward; the Strata shell source itself is already at the desired smoother-drag revision. I’m fast-forwarding those support files, then validating the local build before activation.

## Assistant (progress) - 08/28/2026 17:18:31

The build passes its full shell self-test on the local drive. Before switching, I’m preserving two things the older developer installer could otherwise disturb: the existing Explorer recovery backup and the current `.exe` uninstaller registration. I’m using the newer maintenance path where possible so this remains a proper installed shell, not just a manually pointed executable.

## Assistant (progress) - 08/28/2026 17:19:54

The local build is validated, and its developer install mode will use all 1,010 wallpapers directly from `C:\Projects\StrataShell\wallpapers` instead of copying the smaller installer library. The protected Windows shell setting will require one normal administrator approval during activation; I’ll trigger that only after the new local release and recovery backup are fully verified.

## Assistant (progress) - 08/28/2026 17:20:34

Recovery is now backed up and the original Explorer state is preserved. I’m publishing the clean local-project build into a new immutable release directory; this step does not yet interrupt the running shell or change the Windows shell target.

## Assistant (progress) - 08/28/2026 17:21:36

The new local release is complete, self-contained, and passes the installed-location self-test. I’m switching the protected shell registration now; Windows may show one UAC confirmation for the verified policy helper. After approval, I’ll hand the running session over and validate the new process before declaring it finished.

## Assistant (progress) - 08/28/2026 17:23:53

The project build is ready and registered as the new shell target. I’m completing the live handoff now, preserving your current settings and switching wallpaper references to the full project library, then I’ll verify the new shell is actually running.

## Assistant (progress) - 08/28/2026 17:24:28

The old shell shut down cleanly—no forced termination. I’m now repointing its saved wallpaper paths to `C:\Projects\StrataShell\wallpapers`, then starting the new project build under the registered shell policy.

## Assistant (progress) - 08/28/2026 17:25:22

The handoff succeeded: both the new bootstrap and the new shell are running from the project-derived release, and your active wallpaper was preserved. I’m doing the final proof now—registry target, visible shell windows, self-test, wallpaper counts, and startup logs.

## Assistant (progress) - 08/28/2026 17:26:07

The live checks passed: the new desktop, rail, and screensaver windows are visible; all 1,010 project wallpapers are available; and every saved wallpaper path resolves. One installer tracking file I prepared was removed or relocated during startup, so I’m reconciling that record before calling the switch complete—the shell itself is running normally.

## Assistant - 08/28/2026 17:27:07

Done — the local project build is now your active Windows shell.

- Running commit: `c335e815`
- Source: [C:\Projects\StrataShell](C:/Projects/StrataShell)
- Both bootstrap and shell processes are running from the new release.
- The old `1.0.1` shell is no longer running, but remains available for rollback/uninstall.
- All settings were preserved.
- Wallpaper library now uses the project folder: 110 light + 900 dark wallpapers.
- Self-test passed, startup completed without errors, and the Desktop, Rail, and Screensaver were verified.
- Your next sign-in will also launch this build automatically.
- Recovery backup: `%USERPROFILE%\AppData\Local\StrataShell\Recovery\deployment-backups\local-main-c335e81-20260828-131959`
- The backlog is also on [GitHub](https://github.com/ManiaxMax/StrataShell/blob/main/docs/STRATA_BACKLOG.md).

<oai-mem-citation>
<citation_entries>
MEMORY.md:28-30|note=[preserved concurrent source work and current key map]
MEMORY.md:35-39|note=[used first party app and verification conventions]
</citation_entries>
<rollout_ids>
01a04569-1218-7620-89c7-253716b45567
01a04567-fe1f-7b91-8966-dfc3d9c6a5df
</rollout_ids>
</oai-mem-citation>

## User - 08/28/2026 23:35:50

Send me the .MD please

## Assistant - 08/28/2026 23:36:03

Here it is: [STRATA_BACKLOG.md](C:/Projects/StrataShell/docs/STRATA_BACKLOG.md)

[View it on GitHub](https://github.com/ManiaxMax/StrataShell/blob/main/docs/STRATA_BACKLOG.md)
