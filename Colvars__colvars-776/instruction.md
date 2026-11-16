# Issue

**Title:** Avoid error in acos and asin with fast-math

## Description

by returning hardcoded values on boundaries -1 and 1

Compared with previous fix #768 this is more radical:
- triggers when fast-math is enabled regardless of architecture (following remarks by @HanatoK )
- does not call the library functions when the argument is "equal" to -1 or 1, but returns hard-coded values.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b2071210ee65`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.