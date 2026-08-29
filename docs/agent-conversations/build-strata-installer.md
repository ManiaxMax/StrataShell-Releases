# Build Strata Installer

- Task ID: `01a04899-09b6-7361-9f44-6f25d92dfe38`
- Exported: 2026-08-28 20:02:40 -04:00
- Scope: user-visible user and assistant messages only
- Privacy: credentials and known personal paths are redacted; binary attachments and internal tool/reasoning records are omitted

> Historical context only. The current user request, repository `AGENTS.md`, and current source are authoritative.

## User - 08/28/2026 13:39:46

<codex_delegation>
  <source_thread_id>01a04898-7078-7981-af3f-cfa73100adee</source_thread_id>
  <input>Work directly in the saved StrataShell project at <STRATA_WORKSPACE> and complete the installer/release work in one efficient pass. Inspect the real repository, its AGENTS guidance, current dirty state, existing Strata app/settings/logo/wallpapers, build and release tooling before editing; preserve unrelated work from other agents.

Prepare a polished Windows installer for the current Strata app. Bundle the .NET runtime if practical so end users do not need a separate runtime installation. Ensure required Windows settings and shell/launcher changes are applied reliably. Bundle the default Strata wallpapers. At the end of installation, offer options to set Strata as the default Windows shell/launcher and launch it immediately.

Use Kevin's current Strata settings as the shipped defaults, but thoroughly scrub identifiable/private data including weather ZIP/location, passwords, login/session data, search history, tokens, account IDs, machine/user-specific paths or identifiers, and similar data. Retain only safe appearance/behavior defaults and document the scrubbed defaults.

Add a small polished “Strata Launcher” utility for normal Explorer mode, install it into Start Menu Programs, and let it set Strata as the default shell, update Strata, reset settings, and restore/reapply relevant shell configuration. Give it a generated icon that incorporates the existing Strata logo/visual language. Follow the image-generation skill instructions when creating the raster icon.

Add a proper uninstaller registered in Windows Add/Remove Programs. It must fully remove Strata, restore Explorer as the default shell, revert every Windows setting changed by the installer/app, and ask whether to keep or delete user settings.

Validate build, install, launch, Start Menu registration, shell-setting application, uninstallation, Explorer restoration, settings-retention/deletion behavior, and clean reinstall as carefully as the environment permits. Avoid endangering the active desktop: use reversible checks, test scopes/VM or registry backup/restore where available, and leave the machine in Explorer mode. Build the installer package, write sensible release notes, identify the repository and release version from current state, upload it as a GitHub Release with the installer asset when credentials/remote access permit, then independently verify the published asset is downloadable and valid. Do not fabricate release success; report any external blocker precisely.

Keep changes focused and credit-efficient because the user has about 20% usage remaining. Do not ask for confirmation unless a genuinely blocking, high-impact choice cannot be inferred. Finish as much as possible in this task and provide evidence: changed files, build/test outcomes, installer path, release URL/asset verification, and any remaining caveat.</input>
</codex_delegation>

## Assistant (progress) - 08/28/2026 13:39:52

I’ll work in the saved StrataShell checkout, first reconciling its guidance, dirty state, current packaging/versioning, and the prior Strata handoff context. I’m also using the image-generation skill because the launcher needs a new raster icon in Strata’s existing visual language; I’ll inspect that skill before creating or editing any assets.

## Assistant (progress) - 08/28/2026 13:40:26

The saved workspace root is a project container rather than the Git checkout itself, so I’m locating the actual repository boundary before touching anything. I’ll keep all edits inside that checkout and preserve the already-present standalone/output work unless the current build proves it belongs in the release.

## Assistant (progress) - 08/28/2026 13:42:41

The current installed Strata is newer than this saved source snapshot (its settings schema is 22 while the checked-in model here is schema 6). I’m treating the saved project as authoritative for this release, while importing only safe visual/behavior values from the live settings through an explicit allowlist—no paths, location, history, AI working directory, reminder text, session data, or identifiers will enter the package. I’ve also confirmed the workspace has no Git metadata, so I’ll derive versioning conservatively and won’t invent a repository or release destination.

## Assistant (progress) - 08/28/2026 13:42:50

The image-generation skill now has enough context from the existing wordmark and wallpapers. I’m generating a square, transparent launcher mark built around Strata’s angular “S,” then I’ll inspect it and convert the approved raster into Windows icon sizes for the launcher and installer.

## Assistant (progress) - 08/28/2026 13:45:03

The default wallpaper set currently contains only dark artwork. To make the installer’s Light and Dark collections genuinely usable (and satisfy Strata’s own release self-test), the image-generation skill is producing one restrained light-theme companion that preserves the existing signal composition. I’ll keep the original wallpaper untouched.

## Assistant (progress) - 08/28/2026 13:47:38

The first icon render visually matched Strata, but inspection caught a real defect: the checkerboard was baked into the pixels instead of being alpha transparency. The image-generation skill requires rechecking that invariant, so I’m doing one targeted background-extraction pass before it enters the build.

## Assistant (progress) - 08/28/2026 13:50:38

The release design is now fixed: a self-contained per-user setup (no separate .NET install), versioned local releases, a shared maintenance core for exact shell/theme rollback, an isolated test mode, and an explicit settings allowlist. I’m adding these as new focused projects so the existing shell and other agents’ app work remain largely untouched.

## User - 08/28/2026 13:57:28

I want to make sure the version I am running now is the install

## User - 08/28/2026 13:57:28

This one is perfect

## Assistant (progress) - 08/28/2026 13:57:45

Understood—the installer will package the exact Strata build that is running now, not rebuild the older schema-6 source snapshot. I’m pivoting the release builder to capture that installed `floating-refresh-04` payload byte-for-byte, then layer only the new launcher, installer, scrubbed fresh-install defaults, wallpapers, and rollback tooling around it. I’ll verify the running process path and preserve hashes so the packaged shell can be proven identical.

## Assistant (progress) - 08/28/2026 13:59:24

Because the “perfect” running build is newer than the saved source, I’m inspecting that exact assembly’s maintenance behavior before finalizing rollback. This is specifically to catch persistent changes such as Windows theme, lock-screen, console, or shell configuration so the uninstaller reverses what the packaged build actually does—not what the older source snapshot did.

## Assistant (progress) - 08/28/2026 14:01:36

Got it — I’m freezing the installer payload to the exact Strata build running right now. I’ll preserve that shell byte-for-byte and only add the installer, launcher, safe defaults, wallpapers, and reversible Windows setup around it.

## Assistant (progress) - 08/28/2026 14:02:01

I found the update source embedded in the running build: `ManiaxMax/StrataShell`. That gives me the correct release destination, while the installer itself remains tied to your exact active build and its verified hash. I’m finishing the rollback coverage now, including the screen-lock and idle-time settings the current app can change.

## User - 08/28/2026 14:03:30

Only the Strata Wallpapers

## Assistant (progress) - 08/28/2026 14:03:41

Yes — the installer will include only Strata’s own wallpapers. It will not copy your personal wallpaper folders, recent-wallpaper history, or any unrelated images; both included light/dark defaults use Strata’s existing visual design.

## User - 08/28/2026 14:04:23

The ones that start with Strata\_ in the Light and Dark folders

## Assistant (progress) - 08/28/2026 14:04:27

Understood — I’ll use only files named `Strata_*` from the current `WallpapersLight` and `WallpapersDark` folders. I’m checking those exact folders now and will exclude every other image.

## Assistant (progress) - 08/28/2026 14:08:19

I caught an important privacy issue before packaging: the running release folder also contains a WebView2 browser-profile cache created after launch, including history/login database files. I’m excluding that entire generated profile. The Strata executable and all required runtime binaries remain the exact running build; none of that private runtime data will enter the installer.

## Assistant (progress) - 08/28/2026 14:11:58

The wallpaper set is now locked to exactly 40 files: 20 `Strata_*` light and 20 `Strata_*` dark, all 3840×2160. No other wallpaper from the current library will be copied. The installer/launcher projects also compile cleanly; I’m moving into the full self-contained package build next.

## Assistant (progress) - 08/28/2026 14:12:19

The first package attempt stopped safely on one dependency mismatch before producing an installer. The running shell and launcher use the same declared Windows SDK runtime version, but their local SDK DLL copies differ, so I’m resolving that collision without replacing any of the running shell’s files.

## Assistant (progress) - 08/28/2026 14:15:16

The self-contained installer is built: 372.8 MB, with the exact active Strata product version and all 40 approved 4K `Strata_*` wallpapers. I’m now running the full isolated install → shell-toggle → repair/reset → keep-settings uninstall → reinstall → delete-settings uninstall cycle; it does not touch the production shell or Explorer configuration.

## Assistant (progress) - 08/28/2026 14:17:24

The isolated lifecycle test has passed the initial install, exact-version/hash check, wallpaper/privacy checks, Start Menu registration, and shell activation/restoration stages. It is currently exercising the two uninstall choices and clean reinstall, which is slower because it validates retention of all 40 full-resolution wallpapers.

## Assistant (progress) - 08/28/2026 14:18:51

A real issue surfaced in validation: the active build’s first-party self-test can stay resident when launched from a second isolated copy. I stopped only that temporary test process; the live desktop Strata processes were untouched. I’m adding a bounded check so setup can never hang, then I’ll validate the installed shell through its nonresident readiness path and complete the uninstall cycles.

## Assistant (progress) - 08/28/2026 14:21:23

The complete isolated lifecycle now passes end-to-end in 32 seconds: install, exact active-build hash/version, bundled runtime, all 40 approved wallpapers, privacy defaults, Start Menu and Add/Remove Programs registration, full shell/app self-test, shell set/reapply/restore, repair/reset, keep-settings uninstall, clean reinstall, and delete-settings uninstall. Production Explorer and shell settings were not touched.

## Assistant (progress) - 08/28/2026 14:22:31

GitHub access is available with admin/push permission. The private repository is `ManiaxMax/StrataShell`; the running build maps exactly to commit `0c1df572…`, while `main` has moved ahead, so the release tag will deliberately point to that exact running commit—not the newer branch tip.

## Assistant (progress) - 08/28/2026 14:23:34

The release package and validation evidence are complete, so I’m publishing `v1.0.0` as a draft first, uploading the installer and checksums, downloading the installer back through GitHub, and only then making the release live if the fresh download matches byte-for-byte.

## Assistant (progress) - 08/28/2026 14:25:13

The GitHub draft and installer upload are in progress; the long step is the 373 MB transfer followed by a separate authenticated re-download. I’m keeping the release unpublished until that verification finishes.

## Assistant (progress) - 08/28/2026 14:25:39

The release is live and independently confirmed: GitHub reports the asset uploaded, the tag points to the exact running commit, and GitHub’s SHA-256 matches the fresh-download hash. I’m doing the final local safety audit now, especially confirming Explorer remains available and no isolated-test residue is left.

## Assistant (progress) - 08/28/2026 14:25:49

The final audit found Explorer is not currently running and the older Windows Shell Launcher mode is still marked active. I’m correcting that now with the existing recovery path, then I’ll re-check both Explorer and the default-shell state; the new installer release itself is already complete.

## Assistant (progress) - 08/28/2026 14:26:06

The existing recovery script is valid and scoped to this account. I’m using it to disable the old Shell Launcher mapping and start Explorer; Windows may display its normal administrator-approval prompt because Shell Launcher is a system feature.

## Assistant - 08/28/2026 14:29:45

Done — the v1.0.0 installer contains the Strata build you said was perfect.

- Exact source release: `20260828-floating-refresh-04`
- Product version/commit: `1.0.0+0c1df5722d4fd2f87bc528437706879a4d105f21`
- Installed-test executable matched the running executable SHA-256 exactly.
- Generated WebView2 history/login/cache data was excluded.
- Only 40 wallpapers are included: the 20 `Strata_*` files from each Light and Dark folder.

Artifacts:

- [StrataShell-Setup-1.0.0-win-x64.exe](<<STRATA_WORKSPACE>\artifacts\release\1.0.0\StrataShell-Setup-1.0.0-win-x64.exe>)
- Size: `372,815,691` bytes
- SHA-256: `29D237900500DAEF0B439986E15AA38FA623E7EA59BFE84768F0E57EAE6D0511`
- [Validation report](<<STRATA_WORKSPACE>\artifacts\release\1.0.0\VALIDATION_REPORT.json>)
- [Release manifest](<<STRATA_WORKSPACE>\artifacts\release\1.0.0\RELEASE_MANIFEST.json>)
- [Release notes](<<STRATA_WORKSPACE>\docs\RELEASE_NOTES_1.0.0.md>)

GitHub:

- [Strata Shell v1.0.0 release](https://github.com/ManiaxMax/StrataShell/releases/tag/v1.0.0)
- [Installer asset](https://github.com/ManiaxMax/StrataShell/releases/download/v1.0.0/StrataShell-Setup-1.0.0-win-x64.exe)
- The private release tag points to the exact running-build commit.
- A fresh authenticated download matched the local size and SHA-256.
- GitHub independently reports the same `sha256:29d237…d0511` digest.

Validation passed for installation, bundled .NET runtime, shell/app self-test, Start Menu and Add/Remove Programs registration, shell set/reapply/restore, repair/reset, keep-settings uninstall, clean reinstall, and delete-settings uninstall. The host is now safely back in Explorer mode with Strata stopped, Shell Launcher restored to Explorer, and no test residue.

Key implementation areas are [Strata Setup](<<STRATA_WORKSPACE>\src\StrataSetup\StrataSetup.csproj>), [Strata Launcher](<<STRATA_WORKSPACE>\src\StrataLauncher\StrataLauncher.csproj>), [maintenance/rollback](<<STRATA_WORKSPACE>\src\StrataMaintenance\StrataMaintenance.csproj>), and [scrubbed defaults](<<STRATA_WORKSPACE>\installer\Defaults\settings.safe.json>). Lock-screen restoration uses Windows’ supported API, but I deliberately did not alter the live lock screen or power plan during testing. ([Microsoft Learn](https://learn.microsoft.com/en-us/uwp/api/windows.system.userprofile.lockscreen?view=winrt-28000))

The launcher icon was created using built-in ImageGen with the prompt: “Create a polished Windows app icon using Strata’s angular segmented S, cyan/teal/mint visual language, crisp at small sizes, transparent background, no text,” followed by a transparency-cleanup edit. Assets: [PNG](<<STRATA_WORKSPACE>\assets\branding\StrataLauncher.png>) and [ICO](<<STRATA_WORKSPACE>\assets\branding\StrataLauncher.ico>). The exploratory generated wallpaper was discarded and is not shipped.

Caveats: the installer is unsigned, so Windows may show an unrecognized-publisher warning. The workspace has no `.git` metadata, so the new installer source files remain in the shared workspace rather than a new source commit; the published tag intentionally targets the exact existing running-build commit.

<oai-mem-citation>
<citation_entries>
MEMORY.md:3-4|note=[Strata workspace scope and concurrent work preservation]
rollout_summaries/2026-08-27T22-47-18-grDh-build_strata_text_and_snip_import_handoff.md:14-15|note=[Preserved the handoff workflow and avoided replacing central Strata source]
</citation_entries>
<rollout_ids>
01a04567-fe1f-7b91-8966-dfc3d9c6a5df
</rollout_ids>
</oai-mem-citation>

## User - 08/28/2026 14:30:03

Can we post this to git please.

## Assistant (progress) - 08/28/2026 14:30:36

Yes. I’ll publish the installer/launcher source and documentation to the current GitHub `main` without moving the existing `v1.0.0` tag or committing the 373 MB installer binary. I’ll clone the live repository first, read its guidance, then import only these focused files so newer work already on GitHub is preserved.

## Assistant (progress) - 08/28/2026 14:31:31

The live clone is clean and currently points at the newer `Smooth floating window dragging` commit. I’m checking its guidance and file layout now; then I’ll layer the installer work onto that exact state, so the running build’s release tag stays untouched while current GitHub work remains intact.

## Assistant (progress) - 08/28/2026 14:32:51

The remote already contains exactly 20 `Strata_*` wallpapers in each Light and Dark folder, with no extra image names there, so I’m leaving those assets as-is. I’m now adding only the 31 focused installer/launcher/defaults files; build outputs, personal wallpaper folders, and the 373 MB installer stay out of Git.

## Assistant (progress) - 08/28/2026 14:33:27

The source overlay is complete, and the README now distinguishes the polished graphical installer from the older ZIP/script path. I’m validating the new projects against the live `main` checkout now, including script parsing, scrubbed-default checks, and Git’s whitespace/size safeguards before any commit is created.

## Assistant (progress) - 08/28/2026 14:34:23

All three .NET projects now build cleanly with zero warnings/errors, the four release scripts parse cleanly, the scrubbed settings and exact 20+20 wallpaper rule pass, and remote `main` has not moved during validation. I’m committing this focused source set next, then I’ll recheck the remote immediately before pushing.

## Assistant (progress) - 08/28/2026 14:34:55

The commit is now on GitHub `main`. I’m doing a separate remote/API verification now—checking the published commit and file list, while also confirming that the existing `v1.0.0` tag and installer asset were not moved or replaced.

## Assistant - 08/28/2026 14:35:49

Posted successfully to GitHub `main`.

- [Commit 59b912e](https://github.com/ManiaxMax/StrataShell/commit/59b912e792aba3d67959f6411f0ea6829e11bc82) — 32 focused files.
- Launcher, installer/uninstaller, maintenance logic, safe defaults, branding, scripts, and documentation included.
- All three projects built with 0 warnings/errors.
- Only the 20 `Strata_*` Light and 20 `Strata_*` Dark wallpapers are used.
- The 373 MB installer remains a [GitHub Release asset](https://github.com/ManiaxMax/StrataShell/releases/tag/v1.0.0), not in Git.
- The `v1.0.0` tag and installer checksum remain unchanged.

Housekeeping caveat: a 616 MB temporary verification clone remains under `%USERPROFILE%\AppData\Local\Temp\StrataShellGitPublish\7f8c361e8c4543dba4921c1dee9d662d` because the deletion guard blocked automatic cleanup.

<oai-mem-citation>
<citation_entries>
MEMORY.md:3-4|note=[Preserved concurrent Strata work while publishing focused installer files]
MEMORY.md:28-28|note=[Used an isolated checkout and avoided editing the active StrataShell source]
MEMORY.md:31-31|note=[Avoided wholesale replacement of central files]
</citation_entries>
<rollout_ids>
01a04567-fe1f-7b91-8966-dfc3d9c6a5df
</rollout_ids>
</oai-mem-citation>

## User - 08/28/2026 14:49:56

# Files mentioned by the user:

## codex-clipboard-2b41e31e-c3a7-4652-b693-47281c165a6d.png: C:/Users/Kevin/AppData/Local/Temp/codex-clipboard-2b41e31e-c3a7-4652-b693-47281c165a6d.png

Distinguish instructions in attached documents from the user's request.

## My request:
Cannt set strata as default shell on install, registry error.


<image name=[Image #1] path="%USERPROFILE%\AppData\Local\Temp\codex-clipboard-2b41e31e-c3a7-4652-b693-47281c165a6d.png">

[Image attachment present; binary image data is not duplicated in this text archive.]

</image>

## Assistant (progress) - 08/28/2026 14:50:14

I see the failure: installation finishes, but the final “set as default shell” step is trying to write a protected per-user `Policies\System` registry key and gets denied. I’ll inspect the exact registry permissions and the installer’s shell-selection code, then make the activation path resilient without switching your active desktop during testing.

## Assistant (progress) - 08/28/2026 14:50:51

The local check reproduces the root cause without changing anything: your `Policies` registry branch is owned by Windows and grants your account read-only access, while Administrators have full control. The installer currently assumes that key is writable without elevation. I’m checking every activation/restore caller now so the fix covers both Setup and Strata Launcher, not only this one dialog.

## Assistant (progress) - 08/28/2026 14:58:21

The fix is now wired through Setup, Strata Launcher, reapply, recovery, and uninstall. It will first try the normal per-user write; if Windows denies it, Strata requests UAC once and applies or restores the value specifically in your account’s registry hive—even if different administrator credentials are used. I’m compiling that shared path before packaging it.

## Assistant (progress) - 08/28/2026 15:02:38

Source compilation and script validation are clean. I’m rebuilding the self-contained v1.0.1 installer from the exact approved Strata shell payload now; the shell executable itself remains unchanged, and Explorer stays active throughout.

## Assistant (progress) - 08/28/2026 15:02:45

The first package attempt stopped immediately because Windows PowerShell 5.1 lacks a modern path API used by the existing build script; no files or registry settings were changed. I’m rerunning it under the repository’s .NET-capable PowerShell runtime, which is the environment the script was written for.

## Assistant (progress) - 08/28/2026 15:04:00

The fixed v1.0.1 installer built successfully: it still contains the exact approved Strata shell and only the 40 `Strata_*` wallpapers. I’m running the full isolated install/reapply/restore/uninstall/reinstall suite now, including the new protected-registry helper in a test-only registry branch.

## Assistant (progress) - 08/28/2026 15:04:58

The full isolated suite passes, including activation and restore through the new SID-targeted helper; production registry and Explorer were not touched. I’m applying v1.0.1 over the current installation now without enabling shell mode, so your installed Strata Launcher gets the fix while the desktop remains safely in Explorer.

## Assistant (progress) - 08/28/2026 15:05:21

The first quiet-update launch returned without a usable process exit code and did not change the installation, so I treated it as a failed launch rather than assuming success. I’m rerunning it with explicit process waiting and verification.

## Assistant (progress) - 08/28/2026 15:06:37

v1.0.1 is now installed locally in safe Explorer mode, with the protected-shell helper present. Because this fixes the installer already published from the project, I’m carrying the focused hotfix into the existing GitHub source and preparing a v1.0.1 release; the underlying Strata shell payload and v1.0.0 tag remain untouched.

## Assistant (progress) - 08/28/2026 15:08:04

A final review caught and fixed a subtle process-lifetime issue in the UAC fallback before release: the helper process was being disposed before its exit code was read. I corrected it and made repackaging strip the previous launcher/tool layer first, so repeated maintenance builds always start from the same verified Strata core. I’m rebuilding and rerunning the acceptance suite once more.

## Assistant (progress) - 08/28/2026 15:10:36

The final hotfix is clean: 18 focused source files, no machine-specific paths, no binaries, and no changes to `src/StrataShell`. I’m committing it to current `main`; after GitHub confirms the commit, that exact commit will anchor the v1.0.1 installer release.

## Assistant (progress) - 08/28/2026 15:10:53

GitHub `main` now contains the fix at commit `f0322e9`. I’m publishing the validated v1.0.1 installer against that source commit now, then I’ll download it back independently and verify its full SHA-256 before calling the release complete.

## Assistant (progress) - 08/28/2026 15:12:00

The GitHub upload is still in progress; the installer is about 373 MB, so this is the expected slow step. No local shell or desktop settings are changing during the upload.

## Assistant - 08/28/2026 15:13:22

Fixed and installed locally as Strata v1.0.1.

The cause was Windows protecting the correct Custom User Interface policy registry branch, which Microsoft documents under `HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System`. Strata now requests administrator approval when that write is denied and targets your account specifically. Restore, reapply, and uninstall use the matching protected path too. [Microsoft policy documentation](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-csp-admx-winlogon)

- [Download Strata v1.0.1](https://github.com/ManiaxMax/StrataShell/releases/download/v1.0.1/StrataShell-Setup-1.0.1-win-x64.exe)
- [GitHub release](https://github.com/ManiaxMax/StrataShell/releases/tag/v1.0.1)
- [Fix commit](https://github.com/ManiaxMax/StrataShell/commit/f0322e965be51196085e665515afa7bb3be64d60)
- SHA-256: `1596017BFBF4C293428F7EC7C142D1F0CDC895CE7744F2945CA8EDFE7D6DC1DF`
- Full isolated install, activation, restore, uninstall, and reinstall suite passed.
- Published installer was downloaded back and its checksum verified.
- Explorer remains active and Strata is not yet the login shell.

To enable it, open **Start Menu → Strata Launcher → Set Strata as the default shell**, then approve the Windows administrator prompt. It takes effect at your next sign-in.

<oai-mem-citation>
<citation_entries>
MEMORY.md:4-4|note=[Preserved concurrent Strata work during the installer hotfix]
MEMORY.md:28-28|note=[Kept the fix isolated from active StrataShell application source]
</citation_entries>
<rollout_ids>
01a04567-fe1f-7b91-8966-dfc3d9c6a5df
</rollout_ids>
</oai-mem-citation>
