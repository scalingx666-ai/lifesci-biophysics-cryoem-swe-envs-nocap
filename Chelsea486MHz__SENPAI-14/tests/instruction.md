# Issue

**Title:** Fix a few uninitialized memory uses

## Description

Fixed a few uninitialized memory uses found by MemorySanitizer

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `52ca7656b828`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.