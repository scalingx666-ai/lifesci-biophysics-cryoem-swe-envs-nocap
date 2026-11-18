# Issue

**Title:** [JAX] Import scipy submodules directly in code that uses them.

## Description

[JAX] Import scipy submodules directly in code that uses them.

JAX will soon import scipy lazily, so code that relies on JAX having already imported submodules of scipy will fail. Change code to import what it uses.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80e33bfe629a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.