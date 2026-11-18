# Issue

**Title:** deprecate nseg and bump version

## Description

Deprecation helper should have raised when bumping the version, but did not trigger. This fixes that, deprecates nseg and bumps the version to `0.6.1`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cc7b8c82e5f7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.