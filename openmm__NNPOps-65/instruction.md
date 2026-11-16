# Issue

**Title:** Refactor the common code

## Description

- [x] Factor out the CUDA accessors
- [x] Factor out `atomicAdd`

Needed to simplify

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `332b2faa1cbe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.