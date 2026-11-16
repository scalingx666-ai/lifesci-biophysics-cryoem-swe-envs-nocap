# Issue

**Title:** Remove compatibility code for NumPy missing

## Description

NumPy has been a required dependency for some time so checks and cases for missing NumPy are no longer needed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f4bb348d5b8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.