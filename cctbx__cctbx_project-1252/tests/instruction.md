# Issue

**Title:** Reduce2 on all models

## Description

This sets the default modelIndex to None rather than 0 in the Optimizer __init__ function, which runs across all models by default. This matches the specified behavior of reduce2.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `286dc07ccb49`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.