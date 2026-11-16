# Issue

**Title:** More gently fail if AmberTools provenance fails

## Description

See  for parallel context, though this is a standalone change

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a93553259b49`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.