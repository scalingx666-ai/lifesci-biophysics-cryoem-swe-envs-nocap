# Issue

**Title:** CompatHelper: bump compat for "DataFrames" to "1.0"

## Description

This pull request changes the compat entry for the `DataFrames` package from `0.19, 0.20, 0.21, 0.22` to `0.19, 0.20, 0.21, 0.22, 1.0`.

This keeps the compat entries for earlier versions.

Note: I have not tested your package with this new compat entry. It is your responsibility to make sure that your package tests pass before you merge this pull request.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1a1274c49ae7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.