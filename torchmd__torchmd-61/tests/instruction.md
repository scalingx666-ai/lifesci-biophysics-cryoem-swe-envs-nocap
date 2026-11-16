# Issue

**Title:** Make masses from elements

## Description

Considering the #56, I'm proposing an update for `make_masses()` to use  `mol.element` instead of the `mol.atomtype` and  `ff`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4c4bca9aca8e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.