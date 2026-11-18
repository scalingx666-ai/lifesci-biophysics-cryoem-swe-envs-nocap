# Issue

**Title:** Add identity transformation

## Description

It seemed clunky to have to use `AffineTransform(scale=1, shift=0)`, so this adds `IdentityTransform()`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c3f949c3cd76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.