# Issue

**Title:** fix bug in python version

## Description

`"==3.12"` does not allow any `"==3.12.x"` versions

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1a05f20ad247`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.