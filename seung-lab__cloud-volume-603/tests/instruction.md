# Issue

**Title:** Bugfix: fixes skeleton viewer

## Description

Recommended way to instantiate Axes3D has led to breaking changes in matplotlib, causing axis to not render. See

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c81a74a3fe6e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.