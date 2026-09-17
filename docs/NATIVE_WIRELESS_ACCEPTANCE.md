# Native Bluetooth and Wi-Fi follow-up

September 5, 2026 · `C:\Projects\StrataShell` · Phase 5 is now active; see [current status](STATUS.md).

## Outcome

Settings and the rail now share STRATA-native wireless controls. Bluetooth appears beside Network in the status/tray group. Clicking Network opens its working menu. The menus appear before hardware discovery completes, and neither routine workflow requires starting Explorer.

Bluetooth lists update as devices arrive, change state or disappear. Cards have device glyphs, readable names, connection state and explicit actions. The native Windows custom-pairing API supports confirmation-only pairing, displaying a code to type on a keyboard, entering a device PIN, and confirming matching codes. These are the standard ceremonies needed by mice, keyboards, headsets, speakers and many other Bluetooth devices. Pairing rejection/cancellation is preserved, and displayed PINs clear after the operation.

Wi-Fi exposes Connect, Disconnect, Forget and an on/off radio toggle. Saved Connections includes profiles outside radio range; forgetting requires confirmation and targets the exact adapter/profile. Managed profiles are protected. Duplicate saved/unsaved observations are consolidated while different adapters and security types remain distinct.

## Root causes corrected

- The old Bluetooth path waited for four complete enumerations, opened paired-device objects to inspect them, then repeated that work three times for Add Device. The new path uses Classic/LE AssociationEndpoint watchers and connection properties, with coalesced incremental updates and bounded storage.
- Pairing/removal now opens the explicit AssociationEndpoint kind. The previous default-kind lookup was inappropriate for an endpoint pairing workflow.
- The network button explicitly returned without opening a menu. Its normal panel event now runs, and Bluetooth has its own button.
- Flyouts previously awaited the full hardware list before showing. They now show immediately, and their controls fill asynchronously.
- Deactivation no longer dismisses a wireless flyout while its password/pairing interaction owns a dialog.
- A paired Bluetooth wrapper alone is not evidence of a connection. Explicit Connect requests uncached service discovery; displayed connection state comes from Windows events. Automatic audio-output selection based only on a similar friendly name was removed.
- Friendly names no longer merge separate Bluetooth identities. Updates preserve card identity where a physical address is available.

## Main files

| Owner | Responsibility |
|---|---|
| `Services/BluetoothDiscoveryService.cs` | Shared, scoped Classic/LE discovery watchers and incremental state |
| `Services/SystemControlService.cs` and `.Wireless.cs` | Pairing, connection requests, radio access, serialized native operations |
| `Services/NativeWifiService.cs` | WLAN radio controls, exact saved-profile deletion, safe metadata and network deduplication |
| `Views/WirelessPanel.cs` | Shared themed cards, actions, PIN prompts, cancellation and lifecycle |
| `Views/QuickPanelWindow.xaml.cs`, `SettingsWindow.xaml.cs`, `TopRailWindow*` | Menu, Settings and tray integration |
| `Services/WirelessAcceptance.cs` | Isolated interaction fixtures, real discovery and menu checks |

No persistent setting was added; schema 39 is unchanged. The obsolete Bluetooth visibility flag remains retired.

## Verification

- Release build: zero warnings and errors.
- **144/144 self-tests passed:** wireless-native-self-test.json (`artifacts/wireless-native-self-test.json`, local evidence).
- **77/77 wireless and Settings checks passed:** wireless-native-final/report.json (`artifacts/wireless-native-final/report.json`, local evidence). Includes incremental results, stable rows, pairing PIN/confirmation/cancel flows, saved reconnect, password enrollment dispatch, disconnect, radio toggles, forget cancellation/exact targeting, missing hardware, identity/security grouping, and all Settings pages.
- The real hardware read-only run reported **nine Bluetooth entries**, with the first device arriving in **71 ms**. Explorer process count was **zero before and after**. These are observations on this host, not a universal discovery-time guarantee. Discovery summary (`artifacts/wireless-native-final/bluetooth-discovery-summary.txt`, local evidence).
- Real menu checks confirm both menus show before enumeration and later display radio state. Screenshots: Bluetooth menu (`artifacts/wireless-native-final/native-Bluetooth-menu.png`, local evidence), Network menu (`artifacts/wireless-native-final/native-Network-menu.png`, local evidence), Dark cards (`artifacts/wireless-native-final/bluetooth-Dark.png`, local evidence), Light cards (`artifacts/wireless-native-final/bluetooth-Light.png`, local evidence), keyboard PIN (`artifacts/wireless-native-final/bluetooth-keyboard-pin.png`, local evidence).
- **485/485 desktop/layout regressions passed:** wireless-desktop-final/report.json (`artifacts/wireless-desktop-final/report.json`, local evidence).

Interaction tests use a controller fixture on a private desktop. Real hardware testing was limited to reading radio state and discovering devices. No real device was paired/removed, no radio was toggled, and no network was disconnected or forgotten during development.

## Remaining acceptance and limits

Install the candidate when authorized, then test a real mouse/keyboard and headset/speaker in pairing mode, including reconnect and audio use. Test Wi-Fi disconnect/reconnect, Forget/Cancel, password entry and off/on. Device drivers and profiles determine which uses Windows supports; a successful pairing or connection request does not prove audio playback or HID input. Bluetooth broadcasts without a pairable endpoint cannot be turned into paired devices by the shell.

Windows retains authentication, protected access prompts and hardware/organization policy. A physical radio switch cannot be bypassed. Hidden/enterprise Wi-Fi still requires a supported Windows or organization-provided profile; unsupported security is never silently treated as open. Explorer recovery and unrelated advanced Windows routes remain available.

## Build state

Changes are local on `preview`, based on `b30fa3e`. The observed running installation is `20260905-080304`; it is the earlier cleanup baseline. This follow-up has not been committed, packaged, installed, restarted or published. No shell policy, Windows permission, Bluetooth association or Wi-Fi profile was changed by validation.

## API references

Streaming endpoint discovery follows Microsoft's [device enumeration guidance](https://learn.microsoft.com/en-us/windows/apps/develop/devices-sensors/enumerate-devices). STRATA's PIN/confirmation UI participates in the supported [custom pairing ceremonies](https://learn.microsoft.com/en-us/windows/apps/develop/devices-sensors/pair-devices); Windows performs the authentication. Wi-Fi controls use [WLAN PHY radio state](https://learn.microsoft.com/en-us/windows/win32/api/wlanapi/ns-wlanapi-wlan_phy_radio_state) and [exact profile deletion](https://learn.microsoft.com/en-us/windows/win32/api/wlanapi/nf-wlanapi-wlandeleteprofile).
