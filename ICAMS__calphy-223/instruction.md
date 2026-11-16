# Issue

**Title:** Report submission failures

## Description

In case the sbatch fails, just print out/err, but don't do anything about it as before.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `333ad4da0ac6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.