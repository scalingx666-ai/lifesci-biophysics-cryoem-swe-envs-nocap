# Issue

**Title:** Use out-of-place scatter_add

## Description

Use out-of-place scatter_add to allow using torch.vmap over evaluate_torsion.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `78063fb38ec0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.