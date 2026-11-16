# Issue

**Title:** Constrain NumPy < 2.3 in tests

## Description

ParmEd does not work with the newest NumPy release, each of which we need in tests

Since this causes CI to fail for reasons we cannot control, I aim to merge this without review.

Since we don't depend on ParmEd for our own features, I don't plan to update the packaging of our packages.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `713cfa90174b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.