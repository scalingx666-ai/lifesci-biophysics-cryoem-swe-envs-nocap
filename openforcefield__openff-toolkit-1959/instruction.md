# Issue

**Title:** Defer/fix some minor import issues

## Description

I'm hunting down some issues with slow import times. Some are likely due to OpenMM sneaking in where it doesn't need to be (might be fixed upstream ) but I found a few other minor improvements when digging.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7f8652c3bbbd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.