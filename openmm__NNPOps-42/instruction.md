# Issue

**Title:** Optimization to SchNet

## Description

In many cases, the majority of radial basis functions are negligible for a given interaction.  This takes advantage of that to skip unnecessary calculations in the first dense layer.  In my tests, it produces about a 20% speedup.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6939536c9958`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.