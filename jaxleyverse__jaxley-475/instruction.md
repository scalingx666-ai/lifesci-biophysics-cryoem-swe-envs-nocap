# Issue

**Title:** Add runtime tests

## Description

Note that the tests are disabled in the workflow: `pytest -m "not runtime"`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35223e17d9b3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.