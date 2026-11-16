# Issue

**Title:** Deleted unreachable/duplicated return statements

## Description

There were a few scripts with double `return` statements in the input parsing function - typo/leftover from copy-paste.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37008a8eb006`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.