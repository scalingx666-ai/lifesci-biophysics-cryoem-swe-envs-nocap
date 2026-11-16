# Issue

**Title:** If check on terms for LJ added

## Description

This "if" avoid to compute the LJ parameters when you have forceterms=[] in your --conf file.yaml. Useful when you don't have prior and want to load a .ckpt from torchmd-net.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ccba99aba3e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.