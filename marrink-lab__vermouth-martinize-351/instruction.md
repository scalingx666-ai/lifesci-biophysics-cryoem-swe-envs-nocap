# Issue

**Title:** Switch PyPI deployment from test to actual

## Description

We'll test this when making the next release, for example when #350 is merged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a98098629ace`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.