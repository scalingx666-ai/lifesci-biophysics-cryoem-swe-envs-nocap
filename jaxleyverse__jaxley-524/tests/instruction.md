# Issue

**Title:** Move `read_swc.py` to `io`

## Description

I think it makes more sense to have the swc reader in a dedicated IO module and not in `modules/cell.py`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5e3637c26e03`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.