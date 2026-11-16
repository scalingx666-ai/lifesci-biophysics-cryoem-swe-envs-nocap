# Issue

**Title:** Adds test for different water models

## Description

* Fixes the tip4p case as well, though probably not a particularly interesting model (it is incompatible with water sampling since it expects water to be of size 3)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3052b28aad22`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.