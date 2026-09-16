# STRATA release signing

STRATA has two kinds of signing. The archive signature lets STRATA recognize its publisher's updates. Windows Authenticode gives downloaded executables a Windows-recognized publisher identity. Batch 1 implements the first; Authenticode still needs an approved certificate or signing service.

## What testers need

Nothing extra. The public verification key ships inside STRATA. Friends do not need a signing account, private key, password or certificate.

Full Stable Setup and portable packages remain installation routes. Lightweight Preview packages reuse a Stable-installed runtime or require a compatible installed .NET runtime; they do not contain a full Setup EXE or wallpaper library.

The first signed package is structurally compatible with 1.0.11/1.0.12. After installing the hardened client, unsigned automatic updates are rejected. Publish a signed Stable package when promoting these changes; an older unsigned Stable rollback will be rejected. Windows unknown-publisher warnings remain until Authenticode is configured. Archive signatures do not override Windows application-control policy.

## Publishing

The public key is src/StrataShell/ReleaseTrustKey.pem. The matching private key is outside the repository at %LOCALAPPDATA%\StrataReleaseSigning\release-key.dpapi, encrypted for the publishing Windows account. Never distribute that private file or commit it.

Run scripts/Initialize-StrataReleaseSigning.ps1 with PowerShell 7 to validate/reuse the setup. It refuses to replace an existing public trust anchor. Release scripts invoke Sign-StrataUpdateArchive.ps1 and validate its output automatically before computing hashes or uploading.

The ZIP includes STRATA_UPDATE_MANIFEST.json and STRATA_UPDATE_MANIFEST.sig. RSA-3072 / SHA-256 / PSS authenticates the exact manifest, release ID and complete file inventory. The client also requires the matching GitHub SHA-256 digest.

A fresh CI machine cannot use this account's DPAPI key. Signing fails closed until an authorized CI signing/key-custody integration exists. Do not commit a private key or disable verification to bypass this.

## Backup and recovery

Create an encrypted portable backup using PowerShell 7 from the source repository:

~~~powershell
.\scripts\Backup-StrataReleaseSigning.ps1 -Mode Export -Path 'E:\PrivateBackups\STRATA-release-key.encrypted-pkcs8'
~~~

Choose an existing private backup directory and a new filename. The script prompts locally for a passphrase of at least 16 characters, encrypts the key, and verifies the encrypted backup before writing it. Store the backup outside the repository and keep the passphrase separately in your password manager. Do not send the passphrase through chat.

Recover on another publishing Windows account with the same checked-in public key:

~~~powershell
.\scripts\Backup-StrataReleaseSigning.ps1 -Mode Import -Path 'E:\PrivateBackups\STRATA-release-key.encrypted-pkcs8'
~~~

Import requires the correct passphrase and matching public identity, protects the key for the new account, and refuses to overwrite an existing key. The original DPAPI file alone is not a portable backup. Losing the matching private key can prevent existing clients accepting future updates. Key rotation needs a deliberate trust-transition release; do not regenerate the public key casually.

## Windows publisher signing (deferred)

Enrollment is deferred and is not required for the current cleanup. The following is reference information for a later authorized signing pass.

One supported route is [Microsoft Artifact Signing](https://learn.microsoft.com/en-us/azure/artifact-signing/overview), a managed service supporting public-trust signing. Follow Microsoft's [setup quickstart](https://learn.microsoft.com/en-us/azure/artifact-signing/quickstart) to establish Azure resources, complete identity validation, and create a Public Trust certificate profile. An approved Authenticode certificate provider is another route.

After enrollment, integrate executable signing and timestamping, then verify signatures on freshly downloaded Setup/payload binaries. Sign executable contents before the ZIP manifest and distribution hashes. No enrollment, purchase or public-trust certificate was created in Batch 1.

## Technical sources

- [Windows ProtectedData](https://learn.microsoft.com/en-us/dotnet/api/system.security.cryptography.protecteddata).
- [WebView2 security guidance](https://learn.microsoft.com/en-us/microsoft-edge/webview2/concepts/security).
- [Chromium CRX3 schema](https://chromium.googlesource.com/chromium/src/+/main/components/crx_file/crx3.proto) and [actual verifier](https://github.com/chromium/chromium/blob/main/components/crx_file/crx_verifier.cc).
