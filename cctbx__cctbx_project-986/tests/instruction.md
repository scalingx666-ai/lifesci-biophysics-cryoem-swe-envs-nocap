# Issue

**Title:** Version 2.4.0 of Reduce2 only outputs a single model if it is only op…

## Description

…timizing a single model. It removes all other models before hydrogen addition, which is much faster.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2cb152daa0c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.