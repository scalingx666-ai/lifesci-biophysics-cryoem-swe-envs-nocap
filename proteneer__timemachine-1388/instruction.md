# Issue

**Title:** Update jax to 0.4.28 to fix memory corruption issue

## Description

See https://github.com/jax-ml/jax/releases/tag/jaxlib-v0.4.28

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `149dd0e9c366`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.