# Issue

**Title:** user-set threshold

## Description

Introduces the option to allow the user to set the threshold for normalization if desired. If no threshold is provided, the default as previously defined is used.

A note: The threshold argument was an option for the `_is_normalized_properly` did have a threshold argument, but wasn't easily accessible by the user it seemed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `84998612cb2b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.