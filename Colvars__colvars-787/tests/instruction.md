# Issue

**Title:** Fix restarting lambda-dynamics with NAMD

## Description

Improved the logic of updating the alchemical parameter, separating two cases:
- upon restarting, the value of the parameter is communicated immediately to the back-end (necessary for the next force  calculation to be correct)
- during MD integration, it is cached to be communicated at the end of time-step to avoid race conditions when changing it during the force calculations.

Both cases are handled by the set_value() function of the alchemical CVC.

Note: This evaded testing because the dynamics was not long enough for the value to change significantly between runs. This means we need a negative test, loading an incompatible state and checking that we get an error.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5cea4f1fd56a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.