# STRATA Shell Release, Promotion, and Publication Playbook

This specification defines the exact, end-to-end procedures for version promotion, branching, testing, packaging, and publishing for the STRATA Shell ecosystem. Any engineer or automated AI model can follow these instructions sequentially to execute Preview or Stable releases reliably without ambiguity.

---

## 1. System Architecture and Repository Model

STRATA Shell utilizes a dual-repository distribution architecture:

### 1.1 Private Source Repository (`StrataShell`)
- **Local Path**: `C:\Projects\StrataShell`
- **Remote**: `https://github.com/ManiaxMax/StrataShell.git` (alias: `origin`)
- **Contents**: Full C# / WPF application source code, native Win32/C++ interop layers, unit and acceptance self-tests, release packaging scripts, signing public key (`ReleaseTrustKey.pem`), and internal documentation.
- **Access Rule**: Strictly private. Never commit personal settings, credentials, private signing keys, machine identifiers, logs, or unreviewed binaries.

### 1.2 Public Distribution & Documentation Repository (`StrataShell-Releases`)
- **Local Path**: `C:\Projects\StrataShell-Releases`
- **Remote**: `https://github.com/ManiaxMax/StrataShell-Releases.git` (alias: `origin`)
- **Contents**: Public product documentation (`docs/`), release notes (`docs/RELEASE_NOTES_*.md`), status reports (`docs/STATUS.md`), user manuals, branding assets (`assets/branding/`), and project governance files (`README.md`, `SECURITY.md`, `CONTRIBUTING.md`, `AGENTS.md`).
- **Release Hosting**: GitHub Releases are published under this repository (`https://github.com/ManiaxMax/StrataShell-Releases/releases`).
- **Access Rule**: Strictly public. Never copy internal application source code, private acceptance logs, or unreviewed development notes into this repository.

---

## 2. Release Tracks and Naming Schemes

| Attribute | Preview Channel | Stable Channel |
|---|---|---|
| **Purpose** | Rapid iteration, dogfooding, and prerelease validation | Production release, general availability |
| **Version Scheme** | Timestamp: `yyyyMMdd-HHmmss` (e.g. `20260916-072119`) | Semantic Versioning: `X.Y.Z` (e.g. `1.0.19`) |
| **Git Tag Format** | `yyyyMMdd-HHmmss` | `vX.Y.Z` (e.g. `v1.0.19`) |
| **Source Branches (`StrataShell`)** | `preview` | `main`, `stable`, `release/stable-X.Y.Z` |
| **Release Branches (`StrataShell-Releases`)** | `preview` | `main`, `stable` |
| **GitHub Release Type** | Prerelease (`prerelease: true`) | Full Release (`prerelease: false`) |
| **Runtime Packaging** | Lightweight (~15–25 MB). Reuses Stable-installed .NET runtime bridge or host system .NET 10 | Self-contained (~170–180 MB). Bundles full .NET 10 Windows x64 runtime (`coreclr.dll`, `hostfxr.dll`) |
| **Wallpapers Included** | None (preserves existing installed wallpaper assets) | Approved pair: `STRATA_LOGO_LIGHT.jpg` and `STRATA_LOGO_DARK.jpg` |
| **Installer Executable** | None (portable archive + bootstrap updater) | Full standalone installer: `StrataShell-Setup-X.Y.Z-win-x64.exe` |
| **Primary Automation Script** | `scripts/Publish-StrataPreviewRelease.ps1` | `scripts/Build-StrataInstaller.ps1`<br>`scripts/Test-StrataInstaller.ps1`<br>`scripts/Publish-StrataGitHubRelease.ps1` |

---

## 3. Environment & Toolchain Prerequisites

Before running release scripts, ensure the environment satisfies:

1. **Operating System**: Windows 11 x64 (Build 26100 or later).
2. **Shell**: PowerShell 7 (`pwsh.exe`) or Windows PowerShell 5.1 with ExecutionPolicy set to `Bypass`.
3. **.NET SDK**: .NET 10 SDK installed (targeting `net10.0-windows10.0.26100.0`, `win-x64`).
4. **Git**: Installed and available in PATH.
5. **Release Signing Key**:
   - The private signing key must exist at `%LOCALAPPDATA%\StrataReleaseSigning\release-key.dpapi`.
   - The corresponding public key is checked into `src/StrataShell/ReleaseTrustKey.pem`.
   - Verified via `scripts/Initialize-StrataReleaseSigning.ps1`.
6. **GitHub Credentials**:
   - An authorized personal access token must be present either in the Windows Credential Manager under `LegacyGeneric:target=git:https://github.com` or in `$env:GITHUB_TOKEN`.
   - Token must have `repo` write permissions for `ManiaxMax/StrataShell-Releases`.
7. **Clean Working Trees**: Both repositories must have clean git status (`git status --porcelain` returns nothing).

---

## 4. Procedure A: Publishing a Preview Release

Use this procedure when releasing development builds to the `preview` channel.

```
                    [DEVELOPMENT ON preview BRANCH]
                                   │
                                   ▼
             1. Compile Release & Run Noninteractive Self-Test
                                   │
                                   ▼
              2. Push Source: StrataShell preview -> origin/preview
                                   │
                                   ▼
              3. Sync Docs & Push: StrataShell-Releases preview -> origin/preview
                                   │
                                   ▼
              4. Execute scripts/Publish-StrataPreviewRelease.ps1
                                   │
                                   ▼
                 [PREVIEW RELEASE PUBLISHED & VERIFIED]
```

### Step A1: Verify Source Build and Self-Test
Run from `C:\Projects\StrataShell`:

```powershell
# Ensure you are on the preview branch
git -C c:\Projects\StrataShell checkout preview

# Compile in Release mode
dotnet build .\src\StrataShell\StrataShell.csproj -c Release --nologo

# Run quiet self-test (must exit with code 0)
.\src\StrataShell\bin\Release\net10.0-windows10.0.26100.0\win-x64\StrataShell.exe --self-test --quiet
```

### Step A2: Push Source Preview Branch
Ensure all changes are committed, then push to GitHub:

```powershell
git -C c:\Projects\StrataShell push origin preview
```

### Step A3: Sync Documentation to Release Repository
From `c:\Projects\StrataShell`:

```powershell
$src = 'c:\Projects\StrataShell'
$rel = 'c:\Projects\StrataShell-Releases'

# Ensure release repo is on preview and updated
git -C $rel checkout preview
git -C $rel pull origin preview

# Copy updated README and documentation
Copy-Item (Join-Path $src 'README.md') (Join-Path $rel 'README.md') -Force
$tracked = git -C $rel ls-files docs/
foreach ($t in $tracked) {
    $sourceFile = Join-Path $src $t
    if (Test-Path $sourceFile) {
        Copy-Item $sourceFile (Join-Path $rel $t) -Force
    }
}

# Commit and push if there are changes
$status = git -C $rel status --porcelain
if ($status) {
    git -C $rel add -A
    git -C $rel commit -m "docs: sync documentation for preview release"
    git -C $rel push origin preview
}
```

### Step A4: Execute Preview Release Publication
Generate a timestamp version and trigger the preview publishing script:

```powershell
$previewVersion = Get-Date -Format 'yyyyMMdd-HHmmss'
powershell -ExecutionPolicy Bypass -File .\scripts\Publish-StrataPreviewRelease.ps1 -Version $previewVersion
```

The script will:
- Publish the lightweight `StrataShell` win-x64 binaries (omitting self-contained runtime).
- Verify runtime compatibility bridges and profile routing.
- Digitally sign the update package using DPAPI private key.
- Generate `RELEASE_MANIFEST.json` and `SHA256SUMS.txt`.
- Upload all assets to GitHub Releases under tag `$previewVersion` marked as `prerelease`.
- Validate fresh authenticated download of the archive.

---

## 5. Procedure B: Engine Promotion & Publishing a Stable Release

Use this procedure when promoting accumulated preview work into a formal Stable release (`X.Y.Z`).

```
              [PREVIEW BRANCH VALIDATED & READY]
                              │
                              ▼
           1. Fast-Forward / Merge preview into main
                              │
                              ▼
           2. Bump Engine Version (4 csproj, GitHubUpdateService, Release Notes)
                              │
                              ▼
           3. Compile Release & Run Noninteractive Self-Test
                              │
                              ▼
           4. Commit: "Promote STRATA Engine to X.Y.Z"
                              │
                              ▼
           5. Align Local Branches: preview, stable, release/stable-X.Y.Z
                              │
                              ▼
           6. Push All Source Branches to origin (main, stable, preview, release/stable-X.Y.Z)
              *** MANDATORY BEFORE PACKAGING ***
                              │
                              ▼
           7. Sync Documentation & Release Notes to StrataShell-Releases
              Push release repo branches: main, stable, preview
                              │
                              ▼
           8. Build Installer Bundle: Build-StrataInstaller.ps1 -Version X.Y.Z
                              │
                              ▼
           9. Validate Installer: Test-StrataInstaller.ps1 -Version X.Y.Z
                              │
                              ▼
          10. Publish Stable Release: Publish-StrataGitHubRelease.ps1 -Version X.Y.Z
                              │
                              ▼
                 [STABLE RELEASE PUBLISHED & VERIFIED]
```

### Step B1: Checkout `main` and Fast-Forward from `preview`
Run from `C:\Projects\StrataShell`:

```powershell
git -C c:\Projects\StrataShell checkout main
git -C c:\Projects\StrataShell pull origin main
git -C c:\Projects\StrataShell merge --ff-only preview
```

### Step B2: Bump Engine Version Across Projects
Let `$NEW_VERSION = "X.Y.Z"` (e.g., `1.0.19`). You must update precisely:

1. **`src/StrataLauncher/StrataLauncher.csproj`**:
   ```xml
   <Version>X.Y.Z</Version>
   ```

2. **`src/StrataMaintenance/StrataMaintenance.csproj`**:
   ```xml
   <Version>X.Y.Z</Version>
   ```

3. **`src/StrataSetup/StrataSetup.csproj`**:
   ```xml
   <Version>X.Y.Z</Version>
   ```

4. **`src/StrataShell/StrataShell.csproj`**:
   ```xml
   <Version>X.Y.Z</Version>
   <AssemblyVersion>X.Y.Z.0</AssemblyVersion>
   <FileVersion>X.Y.Z.0</FileVersion>
   ```

5. **`src/StrataShell/Services/GitHubUpdateService.cs`**:
   - Update constant in `RunRecordedInstallationSelfTest()`:
     ```csharp
     const string stableRelease = "X.Y.Z";
     ```
   - Update assertion in `RunSelfTest()`:
     ```csharp
     var stableInstallRetainedAcrossPreview = SelectCurrentRelease(
         "20260830-022417", "X.Y.Z", true, "UNKNOWN") == "X.Y.Z";
     ```

6. **`docs/RELEASE_NOTES_X.Y.Z.md`**:
   Create a complete markdown release notes file detailing features, improvements, fixes, and package specifications.

### Step B3: Compile and Validate Self-Test
```powershell
# Rebuild in Release mode
dotnet build .\src\StrataShell\StrataShell.csproj -c Release --nologo

# Execute self-test suite
.\src\StrataShell\bin\Release\net10.0-windows10.0.26100.0\win-x64\StrataShell.exe --self-test --quiet
```
Ensure exit code is 0.

### Step B4: Commit Version Promotion
```powershell
git -C c:\Projects\StrataShell add `
    src/StrataLauncher/StrataLauncher.csproj `
    src/StrataMaintenance/StrataMaintenance.csproj `
    src/StrataSetup/StrataSetup.csproj `
    src/StrataShell/StrataShell.csproj `
    src/StrataShell/Services/GitHubUpdateService.cs `
    docs/RELEASE_NOTES_X.Y.Z.md

git -C c:\Projects\StrataShell commit -m "Promote STRATA Engine to X.Y.Z"
```

### Step B5: Align Local Branch Pointers
Set `preview`, `stable`, and a new release branch `release/stable-X.Y.Z` to match `main`:

```powershell
git -C c:\Projects\StrataShell branch -f preview main
git -C c:\Projects\StrataShell branch -f stable main
git -C c:\Projects\StrataShell branch -f release/stable-X.Y.Z main
```

### Step B6: Push All Source Branches to GitHub
> [!IMPORTANT]
> `Build-StrataInstaller.ps1` queries `git ls-remote origin refs/heads/main` to confirm that the local commit is already published to the remote `main` branch before building. You MUST push to `origin` before running the installer builder!

```powershell
git -C c:\Projects\StrataShell push origin main stable preview release/stable-X.Y.Z
```

### Step B7: Synchronize and Push Release Repository
Update `StrataShell-Releases` with all documentation, release notes, and assets:

```powershell
$src = 'c:\Projects\StrataShell'
$rel = 'c:\Projects\StrataShell-Releases'

# Checkout main in release repo
git -C $rel checkout main
git -C $rel pull origin main

# Fast-forward main if preview had newer commits
git -C $rel merge --ff-only preview

# Copy README
Copy-Item (Join-Path $src 'README.md') (Join-Path $rel 'README.md') -Force

# Copy all RELEASE_NOTES_*.md
Get-ChildItem -Path (Join-Path $src 'docs') -Filter 'RELEASE_NOTES_*.md' | ForEach-Object {
    Copy-Item $_.FullName (Join-Path $rel "docs\$($_.Name)") -Force
}

# Copy all other tracked doc files
$tracked = git -C $rel ls-files docs/
foreach ($t in $tracked) {
    $sourceFile = Join-Path $src $t
    if (Test-Path $sourceFile) {
        Copy-Item $sourceFile (Join-Path $rel $t) -Force
    }
}

# Copy new docs (e.g. SPHERE.md, SPHERED.md, RELEASE_WORKFLOW.md)
foreach ($doc in @('SPHERE.md', 'SPHERED.md', 'RELEASE_WORKFLOW.md')) {
    $sp = Join-Path $src "docs\$doc"
    if (Test-Path $sp) {
        Copy-Item $sp (Join-Path $rel "docs\$doc") -Force
    }
}

# Commit and update all release repo branches
git -C $rel add -A
$status = git -C $rel status --porcelain
if ($status) {
    git -C $rel commit -m "docs: sync documentation, release notes and guides for X.Y.Z"
}

git -C $rel branch -f stable main
git -C $rel branch -f preview main
git -C $rel push origin main stable preview
```

### Step B8: Build the Stable Installer Bundle
Run the installer build script from `c:\Projects\StrataShell`:

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\Build-StrataInstaller.ps1 -Version X.Y.Z
```

This will:
1. Verify git checkout is clean and matches `origin/main`.
2. Verify all 4 `.csproj` files match version `X.Y.Z`.
3. Publish `StrataShell` and `StrataLauncher` with `--self-contained true` bundling .NET 10.
4. Copy approved wallpaper assets (`STRATA_LOGO_LIGHT.jpg` and `STRATA_LOGO_DARK.jpg`).
5. Build `StrataShell-X.Y.Z.zip` and sign using the DPAPI private key.
6. Compile the standalone setup executable: `StrataShell-Setup-X.Y.Z-win-x64.exe`.
7. Output assets into `C:\Projects\StrataShell\artifacts\release\X.Y.Z\`.

### Step B9: Validate Installer Bundle
Validate the generated bundle in an isolated sandbox environment:

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\Test-StrataInstaller.ps1 -Version X.Y.Z
```

This will:
- Execute isolated silent installation in a Windows temporary directory.
- Verify assembly identities, signatures, and runtime presence.
- Generate `VALIDATION_REPORT.json` with `Passed: true`.

### Step B10: Publish Stable Release to GitHub
Upload the signed release packages to GitHub Releases:

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\Publish-StrataGitHubRelease.ps1 -Version X.Y.Z
```

This script will:
1. Validate `RELEASE_MANIFEST.json` and `VALIDATION_REPORT.json`.
2. Connect to GitHub API via Windows Credential Manager.
3. Create a draft release with tag `vX.Y.Z` on `ManiaxMax/StrataShell-Releases`.
4. Stream and upload assets:
   - `StrataShell-Setup-X.Y.Z-win-x64.exe`
   - `StrataShell-Setup-X.Y.Z-win-x64.exe.sha256`
   - `StrataShell-X.Y.Z.zip`
   - `SHA256SUMS.txt`
   - `RELEASE_MANIFEST.json`
   - `VALIDATION_REPORT.json`
5. Publish release (`draft: false`, `prerelease: false`).
6. Perform an authenticated round-trip download test verifying all SHA-256 hashes match.

---

## 6. Verification and Troubleshooting Reference

### Pre-Flight Verification Checklist
| Item | Verification Command | Expected Output |
|---|---|---|
| Source Git Status | `git -C c:\Projects\StrataShell status -sb` | `## main...origin/main` (Clean) |
| Release Git Status | `git -C c:\Projects\StrataShell-Releases status -sb` | `## main...origin/main` (Clean) |
| Engine Version | `Get-Content src\StrataShell\StrataShell.csproj \| Select-String '<Version>'` | `<Version>X.Y.Z</Version>` |
| Test Suite | `.\src\StrataShell\bin\Release\net10.0-windows10.0.26100.0\win-x64\StrataShell.exe --self-test --quiet` | Exit code 0 |
| Signing Key | `powershell -File .\scripts\Initialize-StrataReleaseSigning.ps1` | DPAPI key valid |

### Common Failure Modes & Solutions

1. **`The local source commit does not match private origin/main`**:
   - *Cause*: `Build-StrataInstaller.ps1` was executed before pushing `main` to `origin`.
   - *Fix*: Run `git -C c:\Projects\StrataShell push origin main` first, then re-run the build script.

2. **`Stable packages must be built from the source repository's main branch`**:
   - *Cause*: Current branch in `StrataShell` is `preview` or another branch.
   - *Fix*: Run `git checkout main`.

3. **`The installer changed after validation` or checksum mismatch**:
   - *Cause*: Files inside `artifacts/release/X.Y.Z` were modified or re-generated without re-running `Test-StrataInstaller.ps1`.
   - *Fix*: Re-run `Test-StrataInstaller.ps1 -Version X.Y.Z` to regenerate `VALIDATION_REPORT.json`.

4. **GitHub Upload Timed Out or Failed**:
   - *Cause*: Network interruption during the 170+ MB upload.
   - *Fix*: Re-running `Publish-StrataGitHubRelease.ps1 -Version X.Y.Z` will detect existing identical assets, re-upload failed ones, and resume publication safely.

5. **`HEAD detached at ...` or untracked `src/` directory**:
   - *Cause*: A git checkout of a remote release branch was attempted inside the source repository.
   - *Fix*: Never add `StrataShell-Releases` as a git remote inside `StrataShell` or checkout foreign branches. Run `git checkout main` or `git checkout preview` to restore the proper source tracking branch. Keep the two directory trees strictly separate.

---

## 7. AI Model Operational Directive (Turnkey Prompts)

Copy and paste these exact instruction blocks to any AI coding agent to execute a release with zero hallucination.

### Prompt Template 1: For Preview Release
```text
Execute a STRATA Shell Preview release according to docs/RELEASE_WORKFLOW.md:
1. Verify source repository (C:\Projects\StrataShell) is on branch 'preview', clean, builds in Release mode, and passes .\src\StrataShell\bin\Release\net10.0-windows10.0.26100.0\win-x64\StrataShell.exe --self-test --quiet.
2. Push source branch 'preview' to origin/preview.
3. Synchronize README.md and docs to C:\Projects\StrataShell-Releases, commit, and push branch 'preview' to origin/preview.
4. Run scripts/Publish-StrataPreviewRelease.ps1 with a timestamp version (yyyyMMdd-HHmmss) to build, sign, and upload the preview release to GitHub Releases.
5. Report the published tag and verified release URL.
```

### Prompt Template 2: For Stable Promotion & Release
```text
Promote STRATA Shell to Stable version X.Y.Z and publish according to docs/RELEASE_WORKFLOW.md:
1. In C:\Projects\StrataShell, checkout 'main' and fast-forward from 'preview'.
2. Bump Engine version to X.Y.Z in:
   - src/StrataLauncher/StrataLauncher.csproj
   - src/StrataMaintenance/StrataMaintenance.csproj
   - src/StrataSetup/StrataSetup.csproj
   - src/StrataShell/StrataShell.csproj (Version, AssemblyVersion, FileVersion)
   - src/StrataShell/Services/GitHubUpdateService.cs (stableRelease constant and self-test assertion)
   - docs/RELEASE_NOTES_X.Y.Z.md (create new release notes)
3. Build in Release mode and verify .\src\StrataShell\bin\Release\net10.0-windows10.0.26100.0\win-x64\StrataShell.exe --self-test --quiet passes with code 0.
4. Commit: "Promote STRATA Engine to X.Y.Z".
5. Align local branches 'preview', 'stable', and 'release/stable-X.Y.Z' to 'main'.
6. Push source branches 'main', 'stable', 'preview', and 'release/stable-X.Y.Z' to origin. (CRITICAL: Push to origin/main BEFORE building installer).
7. In C:\Projects\StrataShell-Releases, checkout 'main', fast-forward, sync all docs/release notes, commit, set 'stable' and 'preview' to 'main', and push 'main', 'stable', 'preview' to origin.
8. Run scripts/Build-StrataInstaller.ps1 -Version X.Y.Z.
9. Run scripts/Test-StrataInstaller.ps1 -Version X.Y.Z (verify Passed: True).
10. Run scripts/Publish-StrataGitHubRelease.ps1 -Version X.Y.Z to upload installer, portable zip, checksums, and manifest to GitHub Releases.
11. Report the published tag, installer checksum, and verified release URL.
```

