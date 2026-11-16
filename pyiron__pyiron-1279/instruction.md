# Issue

**Title:** Use matplotlib-base rather than matplotlib in conda environment

## Description

We do not need `pyqt` so using `matplotlib-base` should be faster: 
https://conda-forge.org/docs/maintainer/knowledge_base.html#matplotlib

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9f5d9f11ddd1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.