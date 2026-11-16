# Issue

**Title:** Pin nbval != 0.10.0

## Description

I'm not sure why we're just now picking up nbval 0.10.0 (released in January), but it seems to be causing tests to fail.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `51cdee310f24`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.