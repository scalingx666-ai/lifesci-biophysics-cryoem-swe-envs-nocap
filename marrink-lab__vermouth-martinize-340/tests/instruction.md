# Issue

**Title:** Remove resid from reference blocks in repair graph

## Description

Turns out that when repairing your graph you can't use blocks from ITP files as reference. Those contain resid numbers/attributes, which will overwrite the resids from your (e.g.) PDB file, causing all sorts of chaos.
This should fix it. I'll also add a test.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `46abb3db1bed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.