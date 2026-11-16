# Issue

**Title:** Update CI to Python 3.15 and remove PowerPC

## Description

Updates the Python 3.13 and 3.14 CI jobs to Python 3.15.  Removes the PowerPC runner as discussed, since we have not built PowerPC packages in a long time, and it is very slow (as the only architecture in CI that has to be emulated).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ef4294d0545e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.