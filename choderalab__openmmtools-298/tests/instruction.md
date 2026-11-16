# Issue

**Title:** Support slice SamplerState with list of indices

## Description

Before this, `SamplerState`s could only be sliced with integer types or slices.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1e998c95ab09`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.