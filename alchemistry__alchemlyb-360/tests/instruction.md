# Issue

**Title:** prepare 2.3.0 release

## Description

pip install -i https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple alchemlyb=="2.3.0-rc1"
Tested and run all the tests fine.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c8fe7b8db0dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.