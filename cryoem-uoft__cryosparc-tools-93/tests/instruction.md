# Issue

**Title:** Refactor: Dataset load memory optimizations

## Description

- New `Dataset.inspect` function for reading dataset headers
- Use chunked mmap loading for numpy arrays
- Break `load` into helper functions

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bd8a6bd2d4a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.