# Issue

**Title:** Fix failure in docs CI build

## Description

cmake.org has started requiring javascript to be enabled, and returns a 403 if it isn't.  That led the link checker to return an error.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b93b58e041ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.