# Issue

**Title:** Remove some print lines and use warnings module where needed.

## Description

Warnings are now also tested in the unit tests. This is related to the discussion in #117.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c4c47c6432bf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.