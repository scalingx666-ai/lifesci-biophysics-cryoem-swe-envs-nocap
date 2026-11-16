# Issue

**Title:** wxtbx.metallicbutton: refresh on EVT_SYS_COLOUR_CHANGED instead of repainting

## Description

## Summary

`MetallicButton.OnChangeSysColor` was directly invoking the private `__DrawButton` method, which constructs `wx.AutoBufferedPaintDCFactory(self)` — on Windows that resolves to `wxPaintDC`. The wx contract requires `wxPaintDC` be constructed only inside an `EVT_PAINT` handler; constructing one from a `wxEVT_SYS_COLOUR_CHANGED` handler trips the `!paintStack.empty()` assertion in `src/msw/dcclient.cpp:260` and aborts the GUI.

The handler fires whenever Windows reports a theme change — light/dark mode toggle, auto-dark switching at sunset, high-contrast on/off, theme refresh during a session unlock or update — so any open GUI window with a `MetallicButton` (used widely as `GradientButton` and `GradButton` across phenix and xfel) crashes.

The fix replaces the in-handler paint with `Refresh()`, so wx schedules a normal `EVT_PAINT` and `OnPaint` calls `__DrawButton` from a legal context. macOS/GTK runtimes are more lenient and don't enforce the assertion, so this is observed only on Windows; the fix is platform-independent.

The existing `if evt:` guard handles the `__init__(evt=None)` construction path unchanged — no paint is needed at construction since the button hasn't been shown yet.

## Test plan

- [x] On macOS: post `wx.SysColourChangedEvent()` to a `MetallicButton`, confirm exactly one `Refresh()` is called and `__DrawButton` is no longer invoked from the handler.
- [ ] On Windows: toggle dark/light mode while a window with a `MetallicButton` is visible; confirm no `wxAssertionError` and the button repaints with the new theme colours.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a1e792f23816`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.