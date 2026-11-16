# Issue

**Title:** Fixed errors in computeCurrentPressure()

## Description

This fixes two problems in `computeCurrentPressure()`.  First, if the system contained any massless molecule it would return nan.  Second, the Python API was not adding units to the return values.

.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b64a82a89a52`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.