# Issue

**Title:** Drop the "beta" label from the Mission Control mode

## Description

## Summary

Mission Control (the meta mode) no longer carries a BETA badge anywhere: not in the Streamlit mode header or upload hero, not in the React welcome screen, mode picker or pre-chat hero. It is the default entry point and has shipped for several releases.

## Technical description

- `scilink/ui/config.py`: the `beta` flag is removed from the meta mode entry; the web `/config` endpoint passes the mode list through, so the field simply disappears from the API.
- `scilink/ui/app.py`: the "Mission Control · …" description prefix keys on `key == "meta"` instead of the flag. `scilink/ui/components/chat_uploads.py`: BETA span removed from the hero title.
- `webui/src`: `beta?` dropped from the `Mode` type; the pill removed from `WelcomeScreen` (two sites) and `PreChatHero`; `.beta-pill` styles removed. `tsc -b` clean; the config endpoint test passes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbf8e2980900`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.