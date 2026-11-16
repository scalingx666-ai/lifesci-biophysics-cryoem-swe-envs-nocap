# Issue

**Title:** Officially drop Python 2 support

## Description

This does not change any of the code, but the documentation and the tests no longer use Python 2 in any way.
In future bug cleanups, we can change all the python 2/3 compatibility things hanging around.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c59162754158`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.