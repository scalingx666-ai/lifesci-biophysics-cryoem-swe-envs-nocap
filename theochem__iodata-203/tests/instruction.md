# Issue

**Title:** Derive development classifier from git tag

## Description

@FarnazH  This change will facilitate making releases. It takes away manual work that one would have to do normally when switching between release types (alpha, beta, stable). This works with the latest version of Roberto 1.11.0.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2d54b7c6a6d5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.