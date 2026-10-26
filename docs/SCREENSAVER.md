# Screensaver

STRATA Shell has an integrated visual screensaver. It is not a Windows `.scr` registration or a secure lock screen. It uses STRATA's current theme colors and shows an animated vector logo across connected monitors.

The fresh-install setting enables it after five minutes of keyboard and mouse inactivity. Change the delay, disable it or start a manual preview in **Settings → UI & Theme → Screensaver**. STRATA Command can also start a preview. Automatic activation defers during presentation or visible fullscreen activity. The preview is the best way to inspect motion and appearance on your own displays.

Mouse movement, a click, wheel, touch or a key dismisses every screensaver surface. The dismissal key is consumed so it does not also reach the app underneath. Session lock, suspend, display changes and shell exit close or rebuild the surfaces as needed.

The catalog contains 30 effects. Reduced Motion uses a calmer presentation; High Contrast uses a crisp logo on black. Beta 20261025-235549 follows each display's reported refresh rate, bounded by WPF composition callbacks, with rounded-timestamp pacing and no separate 30/60 FPS ceiling. Stable 1.0.25 retains the earlier render cap. The shared timeline, all 30 animations and particle density are preserved. Actual displayed frame rate and appearance still depend on the installed build and hardware.

STRATA reads Windows idle, display-off, standby and lock settings for status, but does not extend their timeouts or change power and lock policy. Windows settings take precedence. See [Settings](SETTINGS.md), [features and limits](FEATURES.md), and [release status](STATUS.md). Changes to this documentation or the GitHub logo do not alter an already downloaded installer.