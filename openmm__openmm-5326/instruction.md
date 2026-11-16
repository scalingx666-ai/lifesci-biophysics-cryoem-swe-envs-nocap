# Issue

**Title:** Don't overwrite atomsWereReordered in scaleCoordinates()

## Description

Fixes an issue reported at https://github.com/openmm/openmm/discussions/5185#discussioncomment-17244825.  Barostats' pressure computations involve multiple calls to `scaleCoordinates()`, so it was possible for `restoreCoordinates()` to miss a reordering.

If you think it is worth adding test cases for the barostats that would have identified this issue, let me know.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `748793762be0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.