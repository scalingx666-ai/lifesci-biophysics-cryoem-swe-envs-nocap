# Issue

**Title:** Update CI versions

## Description

- [x] Add PyTorch 1.13
- [ ] ~~Drop CUDA 10.2~~
- [x] Drop Python 3.7
- [x] Update to Ubuntu 22.04

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `16543f913b23`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.