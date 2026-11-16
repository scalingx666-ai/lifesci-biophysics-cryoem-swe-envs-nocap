# Issue

**Title:** Temporary pin: numpy<1.24

## Description

Looks like recent failures in CI are because of a numpy/numba incompatibility. See  Should be fixed soon, but hopefully this pin will fix for now.

.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `609260ab7515`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.