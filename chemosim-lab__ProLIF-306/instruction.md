# Issue

**Title:** Unpin numpy

## Description

Numpy was pinned to `numpy<2` in ea08ac125d7cf9f2b6b133da348deae9bfbe534b with the comment  `# remove numpy<2 when MDAnalysis 2.8.0 is released` which it now has.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `185460c01e06`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.