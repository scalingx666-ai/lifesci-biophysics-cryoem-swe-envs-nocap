# Issue

**Title:** Fullhelper

## Description

I changed the workflow so that instead of writing a backup file matching the original source code and overwriting the source file, the program just writes a 'fullhelp-test' prefixed file that can be renamed to the name of the source file (thereby overwriting it) when you are satisfied that the test looks the way you want it to. Also updated the fullhelp string for fullhelper to reflect this point.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d851007ea6a2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.