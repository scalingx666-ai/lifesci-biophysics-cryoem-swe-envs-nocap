# Issue

**Title:** ci: python 3.13 support

## Description

Note that latest pypa/cibuildwheel we were using was already creating Python 3.13 builds for the latest releases.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0e4521bb8973`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.