# Issue

**Title:** Add Python 3.8, 3.9 to testing matrix

## Description

OpenMM and OpenMMTools are now available on conda-forge (and therefore available for Python > 3.7). .

Because we release via conda-forge, we already release on 3.8 and 3.9, but they haven't been in our test matrix on CI.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f8603273871`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.