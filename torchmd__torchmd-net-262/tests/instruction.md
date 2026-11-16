# Issue

**Title:** Unique COMP6 GDB molecule IDs

## Description

Tiny fix.

The molecule ids were not unique without the name of the file (they are repeated in different files of the same dataset) and it was causing issues in the evaluation when calculating relative conformer energies.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e113eb78908`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.