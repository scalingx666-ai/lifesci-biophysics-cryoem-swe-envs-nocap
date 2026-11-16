# Issue

**Title:** Fast computation of the energy matrix

## Description

This incorporates the changes in choderalab/openmmtools#320 to address #678 .

The Travis tests can only pass once the new version of `openmmtools` is released.

Before merging, I still have to do the following:
- [x] Bump minimum required version of `openmmtools`.
- [x] Update the `whatsnew` file and the YAML docs with these changes and the changes in #880 .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d5cf3663474`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.