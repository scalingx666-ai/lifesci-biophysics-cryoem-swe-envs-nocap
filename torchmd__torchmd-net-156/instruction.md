# Issue

**Title:** Don't store tensors in init args

## Description

On some computers (but not others) this leads to an error when resuming training, due to tensors stored in `hparams.yaml`.  I'm not sure why it only happens on some computers.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0a3534fb2837`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.