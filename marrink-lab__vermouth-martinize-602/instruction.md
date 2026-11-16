# Issue

**Title:** fix error raising for mod not found

## Description

need to reset extra for every resspec, otherwise 'success': False gets appended for every non-find and 'success': True is never added. This only actually affected the error raised, the annotation was actually applied.

another fix for #553

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7e59d59fdc86`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.