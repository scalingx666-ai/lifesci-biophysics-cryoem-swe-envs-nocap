# Issue

**Title:** Skip Changelog test on push to main

## Description

amends #537. Only run changelog on PRs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e955973ff6a7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.