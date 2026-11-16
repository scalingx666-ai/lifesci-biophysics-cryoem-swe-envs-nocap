# Issue

**Title:** CompatHelper: bump compat for GLMakie in [weakdeps] to 0.12, (keep existing compat)

## Description

This pull request changes the compat entry for the `GLMakie` package from `0.8, 0.9, 0.10, 0.11` to `0.8, 0.9, 0.10, 0.11, 0.12`.
This keeps the compat entries for earlier versions.

Note: I have not tested your package with this new compat entry.
It is your responsibility to make sure that your package tests pass before you merge this pull request.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0426f3602246`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.