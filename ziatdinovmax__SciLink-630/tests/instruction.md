# Issue

**Title:** Web UI: lock the sidebar while a session is being created

## Description

## Summary

Pressing **Start Session** left the button, the Resume button and every config field clickable while the app showed "Initializing agent…". A second press could fire a second create request. The sidebar now freezes as soon as a start or restore is in flight: the two buttons turn into a disabled "Starting…" / "Restoring…", the fields lock, and everything unlocks when the request settles (into the session, or back to the form with the error). Nothing else about the flow changes.

## Technical description

- `webui/src/App.tsx`: `startSession` ignores a call while `busy` is set (belt and braces for keyboard or programmatic submits), and passes `starting={busy !== null}` to the sidebar.
- `webui/src/components/Sidebar.tsx`: new optional `starting` prop; `const frozen = locked || starting` replaces `disabled={locked}` on the 14 config fields; the Start button is `disabled={starting || !consent || !effectiveModel}` with the in-flight label, the Resume select and button likewise. The `!locked` gate that shows the form is unchanged, so the form stays visible (and greyed) during the window rather than disappearing.
- The main-pane overlay (`WelcomeScreen`, `busy`) already hid the mode picker during creation; only the sidebar lacked the lock.

Verification: `npm run build` (tsc + vite) passes. Live in Chrome against `scilink serve` on loopback with a Bedrock model: sampled every 100 ms after the click, the button read `disabled=true, "Starting…"` and the model select was disabled from the first sample until the session view replaced the form; exactly one session directory was created.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e9d09915e1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.