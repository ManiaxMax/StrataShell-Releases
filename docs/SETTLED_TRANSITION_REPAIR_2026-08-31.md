# Settled transition repair — 2026-08-31

## Goal and observed baseline

The live walkthrough established native Edge windows as the acceptance baseline:

- one stable composition throughout workspace travel;
- smooth opacity progression without content resampling;
- no wallpaper or backdrop pixels travelling with the window;
- no blank/backdrop-only first frame;
- active-window highlighting absent during motion and restored only after final geometry and focus settle.

The repair targets the shared shell infrastructure so Files, Terminal, Text, Paint, Snip, Image Viewer, Video Player, Settings, Wallpaper Chooser, and other managed STRATA surfaces follow that same lifecycle rather than receiving per-app patches.

## Implementation

- `src/StrataShell/Services/WindowPresentationTransactionPolicy.cs`
  - Adds overlap-safe presentation leases and latest-generation authority.
  - Older valid transactions may finish last but cannot reselect their stale preferred HWND.
- `src/StrataShell/Services/WindowManagerService.cs`
  - Suppresses the active border from transaction start through final placement, retile, focus handoff, close handoff, and floating geometry reconciliation.
  - Covers animated and immediate workspace/view changes, slot swaps, expanded/fullscreen restore, floating/tiled presentation changes, opening/closing fades, and floating move/resize release.
  - Makes opening, closing, top-level movement, and interactive dragging exclusive opacity owners.
  - Cancels floating leases when a window is destroyed, closed, or released to a tray/background process.
- `src/StrataShell/Views/ActiveWindowBorderOverlay.cs`
  - Prepositions the overlay while hidden and only then reveals it.
  - Rejects stale DWM extended-frame bounds that disagree with the current physical window rectangle.
- `src/StrataShell/Services/ManagedWindowAnimationVisualPolicy.cs`
  - Removes desktop screenshot capture from managed-window transitions.
  - Fades the live post-material composite, pauses expensive frost/bloom/effects during geometry motion, and restores them at settle.
  - Avoids pre-hiding reduced-motion openings.
- `src/StrataShell/Services/SurfaceAnimator.cs`
  - Reveals the complete post-acrylic composite instead of only the inner root.
  - Pre-hides already initialized `Hide()`/`Show()` transients before showing them again and uses a one-shot first-initialization handler.
- `src/StrataShell/Interop/FrostedWallpaperBackdrop.cs`
  - Revalidates the wallpaper/strength/monitor texture key after animation.
  - Never republishes a stale frost texture; an uncached replacement uses the non-stale neutral material until it is ready and then fades in over 120 ms.
- `src/StrataShell/Services/HotkeyService.cs`
  - Detects a physical Super press that a silent hook did not observe, waits for chord release, and reinstalls the router.
  - Cancels recovery if a normal callback catches up and removes the blind periodic unhook/rehook window that could itself miss a chord.
- `src/StrataShell/Services/WindowTransitionOpacityPolicy.cs`
  - Defines capture-free workspace and relayout fades plus exclusive opening ownership.
- `src/StrataShell/Services/ShellRecoveryService.cs`
  - Registers deterministic presentation-transaction and silent-router recovery checks.
- `README.md`, `docs/STRATA_BACKLOG.md`, and `docs/QA_RUNTIME_INTERACTION_AUDIT_2026-08-31.md`
  - Replace the obsolete frozen desktop-frame description with the capture-free live-composite policy and record current evidence.

## Verification completed

- Debug `StrataShell` build: passed, 0 warnings, 0 errors.
- Release builds: `StrataShell`, `StrataLauncher`, `StrataSetup`, and `StrataMaintenance` all passed, 0 warnings, 0 errors.
- Hidden deterministic self-test: exit 0, `Success=True`, 84/84 checks passed.
  - Includes overlap/stale generation, close/focus handoff, floating reconcile, reduced-motion opening, capture-free composite, stale frost-key rejection, hidden overlay prepositioning, DWM-bound rejection, and hook catch-up/recovery.
- Full isolated critical UI run from the source build: exit 0 and `passed=True`.
  - Files, Wallpaper Chooser, rail/Snip action, all widgets, launcher, keybind/router, Settings (17/17 sections), Terminal, Text, and Snip passed.
- Two independent Sol Ultra read-only regression reviews found no remaining concrete blocker in the repaired highlight transaction, hotkey, first-party opacity, transient reveal, floating cleanup, or frost publication paths.
- `git diff --check`: passed.

## Remaining runtime acceptance

No installed binary, shell policy, sign-out state, or default-shell activation was changed during this pass. Source validation was isolated and reversible.

The remaining check is installed Preview video/gesture acceptance on the real desktop compositor. Repeat the live Edge baseline and mixed native/STRATA matrix at normal and rapid input rates with blur/bloom combinations: fullscreen and full-fullscreen cycling, `Super + D`, workspace travel and transfer, top/bottom swap, first-party open/close, floating move/resize release, and rapid Super chords. Confirm no centered line/box, stale outline, wallpaper carry, text scaling, backdrop-only frame, opacity pop, lease leak, or Windows-shortcut breakthrough.

## Active-border visibility follow-up

Installed Preview acceptance found that the active border could remain invisible after otherwise-correct settled transitions. Runtime diagnostics confirmed the active target and native border request were correct and every presentation transaction returned to zero; the failure was confined to the fallback overlay reveal path.

- The overlay now reapplies and verifies its physical bounds after WPF `Show()` so WPF startup geometry cannot replace the hidden pre-position.
- A stable outer window rectangle is used when DWM's extended-frame sample is stale or incompatible, preventing permanent suppression while preserving the existing two-frame/70 ms motion gate.
- The post-transition settle timer retries bounded transient motion or presentation rejection instead of silently abandoning the reveal after one tick.
- Deterministic coverage now exercises post-show placement, DWM fallback, bounded retry, ready-target reveal, and supersession behavior.
