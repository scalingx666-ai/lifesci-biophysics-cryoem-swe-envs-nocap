# Issue

**Title:** Numpy fix

## Description

Monkey patch numpy to fix disappearance of _validate_lengths, which affects scikit-image.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `29038c15aeac`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.