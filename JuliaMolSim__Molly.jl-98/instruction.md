# Issue

**Title:** CompatHelper: bump compat for CellListMap to 0.8, (keep existing compat)

## Description

This pull request changes the compat entry for the `CellListMap` package from `0.7.21` to `0.7.21, 0.8`.
This keeps the compat entries for earlier versions.

Note: I have not tested your package with this new compat entry.
It is your responsibility to make sure that your package tests pass before you merge this pull request.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `20642c3ad0c3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.