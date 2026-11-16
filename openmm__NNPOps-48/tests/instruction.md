# Issue

**Title:** Enable CPU-only builds

## Description

- [x] Add `ENABLE_CUDA` option
- [x] Conditionally build the library and PyTorch wrappers
- [x] Update CI

Needed by

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `704117273480`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.