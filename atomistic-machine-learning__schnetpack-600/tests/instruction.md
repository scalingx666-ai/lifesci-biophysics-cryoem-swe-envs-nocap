# Issue

**Title:** Ignore model hyperparameters when saving because they are saved at checkpointing

## Description

This avoids a warning during training, and probably saves some disk space.
I just ran a training and afterwards an MD with this patch applied and didn't observe problems.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37136a1ddde8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.