# Batch 1: safety and trust acceptance

Date: 2026-09-04. Approved scope: S1-S4, D1-D4, R1, and truthful Task Manager readings. Starting source: Engine 1.0.12, commit 98eff4ba7eac4f19579df90e98e64d51502b7a49. Preview publication was requested after local verification. This records source acceptance, not installed physical interactions or laptop performance.

## Implemented behavior

| Audit item | Change | Focused evidence |
| --- | --- | --- |
| S1: extension packages | Verify CRX3 RSA/ECDSA developer signatures and requested store identity; bounded downloads; validate archives before staging; native permission consent; preserve the previous folder on failure. | Valid, tampered, wrong-ID, malformed-header, traversal and failed-staging fixtures. Real Bitwarden Chrome Store 2026.8.0 verified without installing. |
| S2: web-to-native trust | Remove extension installation from web messages. Start-page search/navigation requires matching sender/current origins and a per-page random token. | Remote-origin, stale-page, forged-token and privileged-message rejection. |
| S3: saved passwords | Encrypt manual passwords with DPAPI CurrentUser; atomically migrate readable legacy data; preserve unreadable stores in read-only mode; require Windows Hello before reveal/copy. | Migration, reopen, encryption and corrupt-store preservation. Physical Hello acceptance pending. |
| S4: update authenticity | Require GitHub SHA-256 plus a signed inventory bound to the release ID and embedded public key; verify every file before extraction/execution. Sign before distribution hashes; reject publication with a missing/mismatched GitHub digest. | Trusted, missing, wrong-release and tampered archive fixtures; actual publisher-key verification; old-client package compatibility. Authenticode enrollment remains separate. |
| D1: transfer state | Capture destination, sources and move intent before background work; serialize transfers; preserve a newer clipboard generation. | Mutating clipboard/source inputs after capture does not redirect the operation. |
| D2: move safety | Preflight links/junctions; only use cross-volume fallback for the specific cross-device error; verify copied inventory before source removal; never recursively delete the original. | Failed copies preserve source files/empty folders; actual junction rejected; alternate streams retained. Two-folder fixtures exercise the cross-volume helper, not physical disks. |
| D3: archives | Shared canonical Windows paths; reject special/link/encrypted entries; enforce entry/expanded-byte/time limits; content-addressed bounded cache; retain download-origin metadata. | Traversal/ADS/device paths, actual-byte limits, cancellation, stale archive replacement, preflight rejection and Zone.Identifier preservation. |
| D4: image saves | Match encoder to extension; finish and flush a temporary file before replacing a document; retain replacement metadata; Paint Save/Discard/Cancel. | JPEG/PNG signatures, unsupported formats, interrupted encoder, collisions and download-origin preservation. |
| R1: display lifecycle | Detach the retiring primary rail close handler before rebuilding displays and bind the replacement. | Real unshown WPF windows exercise detach/rebind/close without launching a shell. |
| Task Manager | Replace fabricated/unsampled measurements with Unavailable; retain accessible observations; fix receive rates and service status; report observed counts and confirmed process exit. | Read-only hardware snapshots and formatting/graph handling. |

## Verification record

- Shell, Launcher, Setup and Maintenance Release builds passed with zero warnings/errors.
- Quiet self-test: **106/106 passed**. Evidence: artifacts/batch1-safety/self-test-final.json.
- Real store CRX: artifacts/batch1-safety/crx-store-check/verification.json. Bitwarden 2026.8.0; 23,144,528 bytes; 267 entries. SHA-256 D1A5942D9C234D03382DA99279932055064CAAE7A32CC9B157940C863165D59F. No extension was loaded into a browser profile.
- Release-key export/import round trip, wrong-passphrase rejection and existing-key preservation passed with an isolated generated fixture key. Evidence: artifacts/batch1-safety/key-backup-verification.json. The actual publisher key was not backed up by that test.
- Layout compatibility: artifacts/batch1-safety/20260904-211505/compatibility.json. Original validation/installer-discovery code from **1.0.11 (8fbb34a)** and **1.0.12 (98eff4b)** accepts the added signature files. The new verifier accepts the signed fixture. This does not certify a live update.
- New/modified PowerShell scripts parse successfully. Whitespace and privacy checks gate publication.
- Publication metadata records the exact tested private Preview commit. Fresh public-package verification follows publication.

## Remaining acceptance and limits

- Physical display docking, unplug/replug, primary-display reassignment and mixed DPI. Broader widget/topology migration is later work.
- Windows Hello approve/cancel/unavailable; native extension consent, successful replacement and failed-load recovery in a live WebView profile.
- Physical cross-volume/network-share transfers, read-only/disk-full/cancellation cases and live Paint Save/Discard/Cancel. A failed folder transfer may leave partial destination files while preserving the source. Concurrent external modifications are not a filesystem transaction.
- Link/junction ancestors, ambiguous dot-segment paths, encrypted archives and unsupported destination metadata are deliberately rejected. Limits: 20,000 archive entries, 2 GiB expanded; extensions: 64 MiB download, 256 MiB expanded.
- Create a durable encrypted backup of the real publisher key before relying on that Windows account long term. See [release signing](RELEASE_SIGNING.md).
- Existing users receive the first signed package through their old updater; stronger verification begins after the hardened client is installed. Hardened clients reject older unsigned updates, including unsigned Stable rollback. Publish a signed target release or explicitly install a trusted package manually.
- Signed archives do not supply a Windows publisher identity. Authenticode enrollment/integration remain open.
- Source verification and Preview publication do not install, activate or restart the shell. Batch 2 performance work and MX450/Iris Xe/60 Hz benchmarks remain outstanding.
