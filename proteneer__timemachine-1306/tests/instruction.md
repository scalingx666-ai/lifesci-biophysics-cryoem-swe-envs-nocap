# Issue

**Title:** Remove test requirements from top level docker

## Description

* Don't need pytest, py3Dmol, etc when running timemachine in docker

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3f37cbd9d182`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.