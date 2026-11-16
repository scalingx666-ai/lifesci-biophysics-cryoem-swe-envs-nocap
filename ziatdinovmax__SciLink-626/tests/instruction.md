# Issue

**Title:** Web UI: fix sidebar-reveal overlap and make the memory enable a toggle

## Description

## Summary for a scientist

Two small polish fixes in the React web UI:

- When the left sidebar is hidden, the little **☰ menu button** no longer
  touches the horizontal line under the tabs — it now floats cleanly above
  it with a bit of breathing room and a soft shadow.
- **Enabling persistent memory** (Skills tab) is now an on/off **toggle
  switch** instead of a checkbox, matching the other toggles in the app.

## Technical

- **`webui/src/app.css`** — `.sidebar-reveal` given a fixed compact size
  (38×30) and raised to `top: 6px` so its bottom edge (36px) clears the
  tab-bar border line (~49px) by ~13px; added a `box-shadow` so it reads as
  a floating chip over the tab content. Previously `top: 10px` with
  text-sized padding put the chip's bottom flush against the line.
- **`webui/src/components/MemoryPanel.tsx`** — the "Enable persistent
  memory" control now uses the shared `.toggle-switch` markup
  (`span.toggle-switch > input[checkbox] + span.track`), the same component
  used for the chat verbose-output toggle, so it renders as a sliding
  switch (grey off → cyan on) with the native checkbox visually hidden.
- **`webui/src/app.css`** — added `.toggle-switch input:disabled + .track`
  (dim + not-allowed) for the case where `SCILINK_MEMORY` in the server
  env locks the switch.

### Verification

`npm run build` (tsc + vite) passes. Verified against the live stylesheet in
Chrome by measuring the real classes: the reveal chip's bottom is at 36px vs.
the tab line at 49.5px (13.5px gap, no overlap), with an opaque background;
the memory control renders as a 36×20 pill with a hidden checkbox, a round
knob that slides 16px, and a grey→cyan track on toggle.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `26df9bcc3467`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.