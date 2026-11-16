# Issue

**Title:** Non periodic path

## Description

Split the computation path of non-periodic cells, which can be much simplified, slightly accelerated, and scale better at the cell list construction step.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `236da1d0236f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.