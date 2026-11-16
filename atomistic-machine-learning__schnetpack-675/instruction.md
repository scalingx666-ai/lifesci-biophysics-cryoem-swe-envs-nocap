# Issue

**Title:** fix dataset split into train/val/test

## Description

There's a bug that only occurs if `data.num_test` is *not* None (it is None by default).

Currently, the indices for train/val/test are only picked from the first `num_train + num_val + num_test` samples in the dataset. For instance, if I set all of the three values to 1, then the only dataset indices considered will be 0, 1, 2.

Also the docstring had some issues.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `47cfa5ee8bae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.