# Issue

**Title:** refactor: update the implementation of COLVARS_SYNC_WARP on HIP

## Description

The new macro definition matches the official implementation of __syncwarp() on new ROCm versions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea1c3b8d6d1c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.