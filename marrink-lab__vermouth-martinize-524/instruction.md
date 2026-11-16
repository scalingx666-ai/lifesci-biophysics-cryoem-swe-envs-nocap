# Issue

**Title:** Fix a typo in pdb.py that causes DSSP to fail

## Description

There is a typo in pdb.py which causes DSSP to fail. #348

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b450b6793e85`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.