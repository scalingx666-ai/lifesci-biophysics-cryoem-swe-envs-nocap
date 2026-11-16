# Issue

**Title:** Add support for charged molecules in TensorNet (total charge)

## Description

Training using total charges should be possible by just setting the charge hyperparameter to True in the yaml file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e5fc011ea677`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.