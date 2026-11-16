# Issue

**Title:** Update CI versions

## Description

We were running CI against very old versions of Python, CUDA, and PyTorch.  This updates it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `05d30d46e353`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.