# Issue

**Title:** Sh/spkpredict

## Description

two small fixes for spkpredict:
- bug: removed hardcoded argument in `WrapperLM`
- detach predicted properties and move to CPU to avoid memory issues

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ed10f19164d0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.