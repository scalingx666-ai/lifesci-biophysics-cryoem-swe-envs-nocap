# Issue

**Title:** Move pre-commit to CI requirements

## Description

* Avoid hard coding a version, which was inconsistent with setup.py, in the dockerfile
* Fixes a typo in the Dockerfile

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `69e9347dfdf9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.