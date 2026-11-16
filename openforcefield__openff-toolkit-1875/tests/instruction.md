# Issue

**Title:** Bump Python version used in beta workflow

## Description

There is some difference(s) between 3.10 and 3.11 that was causing a type checker to fail in the beta workflow but not normal CI. This syncs them up.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `60abeb473c28`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.