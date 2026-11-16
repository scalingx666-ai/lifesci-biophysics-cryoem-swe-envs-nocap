# Issue

**Title:** Actually use the EMA loss in `ReduceLROnPlateau`

## Description

At the moment the scheduler `ReduceLROnPlateau` calculates an EMA loss of the given metric but afterwards the unsmoothed metric is passed to the parent class.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b8f810c7a945`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.