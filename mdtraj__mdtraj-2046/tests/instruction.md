# Issue

**Title:** Dropping Python 3.10

## Description

Dropping support for python 3.10, as discussed.

* Change pin in setup.py
* Remove from tests
* Edit doc references to py 3.10

Added:
* SPEC0 badge to README.md

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c66f8f901a53`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.