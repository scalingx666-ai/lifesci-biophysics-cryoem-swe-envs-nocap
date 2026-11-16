# Issue

**Title:** fix: correct stream implementation when dataset is empty

## Description

When saving and loading a dataset. Fixes an error that occurs when saving an external output.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6054fe914dc4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.