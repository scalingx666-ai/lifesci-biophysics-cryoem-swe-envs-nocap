# Issue

**Title:** Subset test created for tools // updated src for v0.2.2

## Description

- Updated docstrings // format // minor bug in `tools.bts.extended_comparisons` esim for release v0.2.2
- Tests were created for `test_esim.py`. All passed
- Tests were created for `test_bts.py`. All passed but `esim` part of `extended_comparison`, should be fixed for this release (v0.2.2)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `76e03281874c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.