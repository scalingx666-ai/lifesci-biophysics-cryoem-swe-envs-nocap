# Issue

**Title:** Grey out the locked sidebar fields and show the live session's settings

## Description

## Summary

Once a session is running, the left sidebar's model, API key, provider, and autonomy fields are now greyed out, as they were in Streamlit. They were already locked, but looked editable.

The locked fields also now show the session you are actually in. Before, a reattached or resumed session displayed the form's defaults ("claude-opus-4-6", "autopilot") even when the session was running on Bedrock in co-pilot mode. Now the model, its provider label and fields (for example the Bedrock region), and the autonomy mode reflect the live session. API keys remain blank, since the server never sends them back to the browser.

## Technical description

### Styling

`webui/src/app.css`: new rules for `input:disabled`, `textarea:disabled`, `select:disabled` (reduced opacity, dimmed text, not-allowed cursor) and for the wrapping `label.field:has(:disabled)` so the caption dims with its control. The same rules apply in both themes through the existing tokens. The consent checkbox is covered by the input rule. Buttons already had a disabled style and are unchanged.

### Locked-field values

`webui/src/components/Sidebar.tsx`: the sidebar already receives the `session` snapshot (it prints the id, model, and autonomy in a caption). Three derived values now drive the controls:

- `shownModel`: the session's model when a session is live, mapped to the "Custom" slot when it is not in the configured list; otherwise the form state.
- `shownCustomModel`: the session's model string in that custom slot; otherwise the form state.
- `shownAutonomy`: the session's autonomy when live; otherwise the form state.

`effectiveModel` is derived from `shownModel`, so the existing config-fetch effect resolves the provider spec for the real model and the key label and provider fields follow it. The form's own state and the start/resume path are untouched: the derivations only change what the disabled controls display. Provider field values (such as region) still come from the form, since the session snapshot does not carry them; when the session was started from this form they are the values used.

Shipped bundle in `scilink/server/static/` rebuilt.

### Verification

Checked in Chrome against a live Bedrock Opus 4.8 co-pilot analyze session: all fields greyed in dark and light themes; the model select shows `bedrock/us.anthropic.claude-opus-4-8`, the key label reads "Bedrock API key" with the "available from AWS_BEARER_TOKEN_BEDROCK" caption, and the AWS region field is present. `tsc -b` clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `30383d693d07`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.