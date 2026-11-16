# Issue

**Title:** Small fixes to docs and to solute-only trajectory

## Description

- Fix #1200 and other small fixes to the documentation.
- Fix #1212, which caused a problem with the stored solute-only trajectories for systems prepared without the automatic pipeline.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `972753ad0f1e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.