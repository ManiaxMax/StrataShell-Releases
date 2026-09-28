# First-party apps

STRATA Shell includes apps for everyday desktop tasks. These are part of the current published shell, but their behavior on a particular device or with third-party software still depends on Windows and the installed build. See [release status](STATUS.md) for verification limits.

| App | Main uses and limits |
|---|---|
| **Browser** | Tabs, bookmarks, history, downloads and per-site permission review using WebView2. Site prompts identify the requesting page; downloads can open in STRATA Files. A page remains subject to WebView2 and website compatibility. |
| **Files** | Browse local, redirected, network and removable locations; use Details or Icons, copy/move, Recycle Bin and supported archive browsing/extraction. Windows shell extensions and every Explorer namespace are not reproduced. |
| **Terminal** | Interactive command sessions and tabs. Elevated sessions use a simpler command mode; full-screen console programs and Ctrl+C are unavailable there. |
| **Notepad** | Edit plain text and Markdown, choose encoding/newline behavior and recover eligible unfinished drafts. It is not a large-file editor; documents above 4 MiB need another app. |
| **Snip** | Capture and annotate screenshots, with save/copy controls. |
| **Paint and Image Viewer** | Basic drawing, image viewing and format-aware saves. |
| **Media Player** | Play supported local media with Windows media components. Protected or unusual codecs and hardware video may require another player. |
| **Calendar** | Keep local appointments shared with the clock widget. It has no online account synchronization. |
| **Task Manager** | Show available process and hardware information. Unsupported, denied or warming-up readings say Unavailable rather than appearing as zero. |

Browser, Files, Terminal and other tabbed surfaces keep crowded tabs reachable by scrolling and revealing the selected tab. Several document apps can open more than one window; utility panels may remain single-instance. The [Settings guide](SETTINGS.md) covers their shared theme and accessibility controls.

## Files permissions and removable drives

For a physical file or folder, Files can open the standard Windows **Security** properties page. Windows performs permission editing and any required elevation. Virtual archive, Computer and Recycle Bin entries do not expose that action.

A removable USB drive may offer **Eject USB device**. Ejection applies to the whole physical device, including its other drive letters. STRATA blocks the request during transfers in that Files window and respects other applications that veto removal; a failed request never reports that removal is safe.

## Browser, credentials and hardware boundaries

Browser site permissions can be reviewed and reset in Browser Settings. Extension installation requires explicit permission and identity checks; downloads do not install extensions automatically. Manual saved passwords are protected for the current Windows account, and reveal/copy uses Windows Hello where supported.

Task Manager uses Windows and available driver or sensor providers. GPU temperature depends on a supported NVIDIA driver; CPU temperature needs an already running compatible sensor provider. STRATA does not install sensor drivers. Missing readings remain unavailable. See [features and limits](FEATURES.md).