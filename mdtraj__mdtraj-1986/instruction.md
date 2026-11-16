# Issue

**Title:** Run openmm tests on CI

## Description

This adds openmm as a dep and also sorts the deps (so then future PRs will have a smaller diff)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `76a7563dc708`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.