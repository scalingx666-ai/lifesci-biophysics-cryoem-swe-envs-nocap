# Issue

**Title:** Catch null dataset_arg

## Description

In the ANI1 example yaml file for example, `dataset_arg` is set to `null`, which causes an error while instantiating the dataset class. This adds a check and ignores the argument if it is `None`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f66ebec5959`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.