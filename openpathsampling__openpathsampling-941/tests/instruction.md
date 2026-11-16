# Issue

**Title:** Fix bug in SimStore when appending to non-existent files

## Description

Pretty much what the title says. Before, there was an error if you opened a new file as append, because it tried to read the non-existent schema from the file.

Also cleans up some deprecations from importing ABCs from `collections`, not `collections.abc` (SimStore is Python 3+.)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fb651045cc42`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.