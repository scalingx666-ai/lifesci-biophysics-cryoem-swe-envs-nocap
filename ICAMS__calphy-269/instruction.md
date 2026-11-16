# Issue

**Title:** Add gres and account fields to scheduler configuration

## Description

I added the `gres` and `account` Slurm flags to the `SLURM` class for easy access, rather than just having to rely on `options`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5c39ae48cc62`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.