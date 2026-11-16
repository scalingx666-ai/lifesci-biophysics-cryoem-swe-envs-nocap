# Issue

**Title:** Cache AllOutX volume

## Description

There is no reason to create a new volume every time we use `AllOutXEnsemble.can_append` etc.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `19318c459987`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.