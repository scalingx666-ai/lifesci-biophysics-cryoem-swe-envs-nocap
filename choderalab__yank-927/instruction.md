# Issue

**Title:** Fix bug in automated Boresch atom picking

## Description

This critical bugfix PR fixes a bug in automated Boresch atom picking where picked atoms were not  being cleared before next attempt.

The bug resulted in YANK never being able to recover from an initial failure.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35bd71884088`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.