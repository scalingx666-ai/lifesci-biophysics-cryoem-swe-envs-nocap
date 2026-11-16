# Issue

**Title:** Make the analysis storage chunk equal to checkpoint interval in SAMS

## Description

This is an optimization for read/write by using the `checkpoint_interval`
for the analysis file, and 1 for the checkpoint file.

Supersedes and  by merging into SAMS

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9172eb9bf033`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.