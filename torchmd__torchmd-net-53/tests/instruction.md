# Issue

**Title:** Make compatible with conda-forge

## Description

In preparation of the NNPOps integration (#50) and further use with OpenMM-Torch, TorchMD-Net dependencies have to be compatible with conda-forge.

- [x] Use a custom PyTorch Geometric package
- [x] Clean up the environment file
- [x] Clean up the CI script

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9a6c4c9a1866`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.