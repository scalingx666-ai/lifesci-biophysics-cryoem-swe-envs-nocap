# Issue

**Title:** fixing dynamic exists

## Description

this function wasn't working for the PCG, as the mesh fragments had multiple : in them, the label is the one before the first :

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e3de12bc232d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.