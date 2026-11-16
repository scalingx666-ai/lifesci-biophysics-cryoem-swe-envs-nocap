# Issue

**Title:** update environment.yml with matplotlib and python 3.7

## Description

RTD fails to build, probably because we added matplotlib to the dependencies.

EDIT: mpl is ok but the Python version needs to be > 3.5 (which I already did with the bump to 3.7)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7be2e410fc23`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.