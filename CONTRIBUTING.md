# Contributing to StrataShell

StrataShell is experimental alpha software and a login-critical Windows component when installed as the default shell. Changes should preserve Floating and Tiled behavior and recovery guarantees.

## Before opening a change

1. Read [Product contract](docs/PRODUCT.md), [Architecture](docs/ARCHITECTURE.md), and [Development and testing](docs/DEVELOPMENT.md).
2. Search existing issues to avoid duplicating work.
3. Keep proposals original. External shells and visual systems may inform design research, but are not asset or source donors.
4. For changes to default-shell activation, recovery, window ownership, or global input, explain the failure and rollback path before implementation.

## Local setup

```powershell
git clone https://github.com/ManiaxMax/StrataShell.git
cd StrataShell
dotnet restore .\src\StrataShell\StrataShell.csproj
dotnet build .\src\StrataShell\StrataShell.csproj -c Release
```

Run normal development work in preview mode:

```powershell
dotnet run --project .\src\StrataShell\StrataShell.csproj -- --preview
```

Do not make a development build the configured Windows shell.

## Project rules

- Preserve the one/two-app Center Stage model and strict two-app workspace capacity.
- Do not restore retired tiler shortcuts or change existing chords silently.
- Keep recovery independent of the Windows-logo key and ordinary shell UI.
- Use visible DWM frame geometry when comparing application width/alignment.
- Keep secure credentials, enrollment, UAC, and sign-in in Windows-owned surfaces.
- Reuse the shared theme resources for first-party UI.
- Avoid high-frequency polling when an event or slower cadence is available.
- Never commit user settings, logs, SIDs, WebView2 profiles, build output, personal wallpapers, or generated releases.
- Do not add third-party assets without clear redistribution rights and attribution.
- Develop changes on a named feature branch. Do not force-push, rewrite, or directly replace `main`; merge only after the feature build has an explicit acceptance path.

## Verification

Every change must at least restore and build cleanly:

```powershell
dotnet restore .\src\StrataShell\StrataShell.csproj
dotnet build .\src\StrataShell\StrataShell.csproj -c Release
```

When the machine has the required installed integrations, also run:

```powershell
.\src\StrataShell\bin\Release\net9.0-windows10.0.26100.0\win-x64\StrataShell.exe --self-test --quiet
```

UI, layout, DWM, theme, widget, or animation changes need before/after screenshots or a short recording and the visual checklist in [Development and testing](docs/DEVELOPMENT.md). Shell/recovery changes need a written preview, activation, crash, and Explorer-return acceptance record.

## Pull requests

Keep each pull request focused. Include:

- the problem and intended user-visible result;
- implementation summary;
- exact checks run and their results;
- screenshots/recording for visible changes;
- recovery and compatibility impact; and
- remaining limitations.

Do not include unrelated formatting, generated output, or personal configuration.

## Licensing

The project does not yet have an open-source license. Public visibility is not permission to redistribute or create derivative works. Until the owner adds a license, discuss substantial outside contributions with the repository owner before investing in them.
