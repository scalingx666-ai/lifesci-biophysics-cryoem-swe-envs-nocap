# Issue

**Title:** Fixed errors installing with pip

## Description

This fixes two errors from #125.  First, it hardcoded a specific version of PyTorch.  Second, it processed CUDA architecture versions incorrectly.  I also fixed a bug that caused `import NNPOps` to fail if torchani wasn't installed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2626033d94c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.