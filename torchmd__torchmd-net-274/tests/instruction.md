# Issue

**Title:** Add dtype parameter to External

## Description

This allows to use External with any dtype.
In addition to being useful for TorchMD integration, I normally use External to put models into CUDA graphs, as it handles all the nuisances transparently.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4601b54127d6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.