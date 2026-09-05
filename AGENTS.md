# Public distribution repository

This repository contains STRATA documentation, approved branding, and compiled release assets. Application source and private development evidence belong exclusively in the private `ManiaxMax/StrataShell` repository.

- Never commit application source, credentials, personal settings, device identifiers, logs, caches or private test artifacts here.
- Keep README, settings, capabilities, defaults and release status consistent. Clearly distinguish current development, published packages and physical acceptance.
- Preserve historical release notes. Do not relabel source tests as installed or visually verified behavior.
- Phase 5 defaults are for new release installs only: Balanced, Floating, the approved widget layout, and Clock/YouTube locked expanded. Updates and shell startup preserve existing choices.
- Stable packages include exactly the approved STRATA_LOGO_DARK.jpg and STRATA_LOGO_LIGHT.jpg wallpaper pair; Preview updates omit the wallpaper library and bundled runtime. Do not add arbitrary wallpapers.
- Release assets must be produced by the reviewed private-source build/publishing workflow. Never fabricate hashes, manifests or validation results.
- `preview` is the prerelease target; `main` is the Stable target. Documentation may update independently with that distinction stated explicitly. Do not publish binaries or promote Stable merely to refresh documentation.
- Preserve unrelated edits. Use an explicit reviewed file list for staging; no force pushes or history rewrites. Commit/push only when authorized by the user or the established release workflow.
- Check local documentation links and logo rendering before declaring the repository refresh complete. Current status belongs in `docs/STATUS.md`; the backlog links there instead of duplicating stale checklists.