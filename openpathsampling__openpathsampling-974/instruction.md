# Issue

**Title:** fix numpy 1.20 DeprecationWarnings

## Description

With the release of `numpy` `1.20` came a couple `DeprecationWarning`s, mainly about `np.float` or `np.bool` just being an alias for the python internal. The one Warning that is left (in my test setup) comes from `openmmtools` and is handled in

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `767d358d7f67`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.