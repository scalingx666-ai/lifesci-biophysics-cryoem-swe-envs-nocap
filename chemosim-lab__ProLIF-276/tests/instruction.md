# Issue

**Title:** Various fixes 2025

## Description

Fixes the absent CI checks noticed by Nilay in #274, plus a few extra things

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `974f5d5aac5d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.