# Issue

**Title:** Lint utilities/ subdirectory

## Description

When working on #1890 I noticed that `utilities/` was not including in the linting checks - this PR does that (and only that) instead of doing it all there

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f74700ac4cfe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.