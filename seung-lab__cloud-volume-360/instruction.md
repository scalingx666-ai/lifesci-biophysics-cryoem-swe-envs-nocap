# Issue

**Title:** fix(precomputed): makeshard cutouts relative to provided image

## Description

`gpts()` returns grid points in dataset space, need to translate them to create correct cutouts from `img`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `46d41419313d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.