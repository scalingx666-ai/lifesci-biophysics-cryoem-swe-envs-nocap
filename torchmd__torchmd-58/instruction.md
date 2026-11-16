# Issue

**Title:** Reshape positions in minimizer to have the required 1D array for scipy

## Description

Latest versions of scipy will raise if the initial guess is not a 1D array. This PR flattens the array before passing it to minimize.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bd447a390334`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.