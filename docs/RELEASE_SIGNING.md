# Update integrity and Windows publisher identity

Current Preview development verifies an RSA-signed update inventory and the release archive's GitHub SHA-256 digest before installation. The public verification key is bundled with STRATA. Testers need no signing account, private key or certificate. Published release notes identify which downloads contain the hardened client.

Windows Authenticode is separate. It supplies a Windows-recognized publisher identity for executable files; archive integrity checks do not suppress Windows unknown-publisher warnings or override application-control policy. Publisher enrollment is deferred and is not a prerequisite for the current cleanup.

Full Stable Setup and portable packages remain installation routes. Lightweight Preview updates reuse the Stable-installed runtime or require a compatible installed .NET runtime; they do not include Setup or the wallpaper library. Once a hardened client is installed, automatic updates must carry a trusted signature. Do not disable validation to install an older unsigned package.

Private signing material, backup instructions and publishing operations are maintained in the private source repository. None belongs in this public repository. See [installation](INSTALLATION.md) and [current release status](STATUS.md).