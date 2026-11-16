# Issue

**Title:** Raise error in spktrain if early stopping metric becomes non-finite

## Description

PyTorch Lightning's EarlyStopping (check_finite=True) stops training silently when the monitored metric becomes NaN or inf. spktrain then continued with testing and model export, and the run was reported as finished. Now spktrain raises a RuntimeError after fit in that case.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ee82f62b8f1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.