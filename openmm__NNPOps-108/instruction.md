# Issue

**Title:** Make some tests use deterministic torch algorithms

## Description

This fixes some tests not passing on some architectures due to numerical inaccuraccies (at least on my machine).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5e2438d06c75`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.