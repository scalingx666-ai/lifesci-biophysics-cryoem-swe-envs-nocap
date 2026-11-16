# Issue

**Title:** Remove dependency on torch_scatter and torch_sparse

## Description

Removes dependency on torch_sparse by implementing message passing operations with manually written ones.

Removes dependency on torch_scatter by using torch.index_add and torch.scatter_reduce.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3ae06ce17113`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.