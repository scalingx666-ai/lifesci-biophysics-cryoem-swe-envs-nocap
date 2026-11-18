# Issue

**Title:** Preload image and mask in EstimateMissingFlow.

## Description

Preload image and mask in EstimateMissingFlow.

This avoids repeated volume reads, which can be slow even when the underlying data is cached in memory (due to cache trashing or need to reassemble the image array out of the underlying chunks).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2ccc66342cca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.