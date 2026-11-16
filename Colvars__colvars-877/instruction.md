# Issue

**Title:** Trap floating-point exceptions

## Description

This detects a couple of divide-by-zero situations that escaped us so far!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e49fa5bc5765`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.