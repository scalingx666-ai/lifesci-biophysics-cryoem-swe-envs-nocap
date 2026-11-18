# Issue

**Title:** redesign: cv.shape returns a tuple + cv.ndim (BREAKING)

## Description

This will provide numpy compatibility, and in turn Dask comaptibility.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a52c128c1061`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.