# Issue

**Title:** Cut Yank 0.18.0

## Description

Due to official Python 2 support dropped, a new major revision is being cut.

Also taking advantage of the new feature in setuptools/pip for `python_requires` + the conda-recipe to force python 3.5 and up. The additional keyword should not cause an issue for pip < 9.0.1

Tests must pass before we can merge this to test the new requirements feature

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ee3f29d97d74`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.