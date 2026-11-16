# Issue

**Title:** Make conda ci use pytest 7.4

## Description

[Conda CI is failing](https://github.com/openforcefield/openff-toolkit/actions/runs/7689310335/job/20951573409) with pytest 8. Until we find the root cause I'm downpinning pytest to 7.4.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cd88bcebeada`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.