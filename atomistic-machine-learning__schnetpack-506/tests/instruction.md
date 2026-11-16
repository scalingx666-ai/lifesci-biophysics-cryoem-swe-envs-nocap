# Issue

**Title:** Updated README and fixed deprecated numpy dtype

## Description

Updated QM9 experiment from `QM9_energy` to `QM9_atomwise` in README and docs.
Fixed casting in `QM9` and `rMD17` datasets which was using the now deprecated `np.int`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3286c478d685`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.