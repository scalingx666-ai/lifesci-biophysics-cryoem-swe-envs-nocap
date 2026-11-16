# Issue

**Title:** Feat: Improved Command error handling and slots errors documentation

## Description

- CommandClient never raises `AssertionError`, only `CommandError`
- More helpful error message when CryoSPARC reports that slot specification is invalid

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0417844a57c7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.