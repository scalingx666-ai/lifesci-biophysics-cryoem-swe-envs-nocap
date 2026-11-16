# Issue

**Title:** Correction to comparison function on ANISOU records

## Description

`ANISOU` records were not being renumbered properly after tidying.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db933d99ba10`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.