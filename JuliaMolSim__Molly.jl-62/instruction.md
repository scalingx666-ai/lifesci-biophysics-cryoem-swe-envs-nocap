# Issue

**Title:** Ensure the termination of function `place_atoms`

## Description

1. Check on maximum atoms that can fit the box.
2. Add kwarg `max_attempts` which decides the termination of loop after this many faliures.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `556db2243368`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.