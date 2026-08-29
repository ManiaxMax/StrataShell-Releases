# Security policy

## Supported versions

StrataShell is experimental alpha software. Only the current `main` branch is maintained; no stable security-support window is promised yet.

## Reporting a vulnerability

Please use GitHub's private vulnerability-reporting feature for this repository if it is enabled. Do not open a public issue for a vulnerability that could:

- break sign-in or create a shell crash loop;
- bypass Explorer recovery;
- execute an unintended command;
- expose local files, credentials, tokens, or account data;
- cross an elevation or secure-desktop boundary; or
- allow untrusted WebView2 or CLI content to escape its intended boundary.

Include the affected commit/build, Windows edition and version, reproduction conditions, observed impact, and whether the issue reproduces in preview or installed-shell mode. Redact account names, SIDs, paths, logs, and secrets that are not necessary to reproduce the issue.

If private reporting is unavailable, contact the repository owner through their GitHub profile and request a private channel before sharing details.

## Operational safety

- Test preview before default-shell activation.
- Keep `%USERPROFILE%\Strata Recovery\Return-To-Explorer.cmd` available.
- Never point Windows Shell Launcher at a network path or mutable development build.
- Do not weaken or remove the installed self-test, Explorer fallback, emergency chord, or bootstrap watchdog to work around a failure.
- Treat AI CLI output, filenames, application metadata, web content, and logs as untrusted input.
- Do not put API keys or credentials in settings, source, screenshots, issues, or diagnostic attachments.

See [Installation and recovery](docs/INSTALLATION.md) for supported activation and rollback.
