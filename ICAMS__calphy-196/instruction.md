# Issue

**Title:** update input to check element ordering explicitely

## Description

This pull request introduces validation logic to ensure the ordering and consistency of element specifications in multi-element systems, particularly focusing on the alignment between the `element`, `mass`, and `pair_coeff` fields. It also adds comprehensive tests to verify these checks and edge cases.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1739be75e420`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.