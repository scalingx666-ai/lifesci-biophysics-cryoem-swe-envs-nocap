# Issue

**Title:** Fix bug in detection logic determining stage

## Description

This fixes a bug in `_update_stage()` where only the iteration where histograms are flat is set to `asymptotically-optimal`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `12c6ec14ae8f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.