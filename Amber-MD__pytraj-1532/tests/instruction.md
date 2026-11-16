# Issue

**Title:** Fixed problem with bonded_indices() method of pytraj.Atom class

## Description

Fix #1531

I changed the dtype of arr from int to i4 and added a test.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1fea8ffd2163`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.