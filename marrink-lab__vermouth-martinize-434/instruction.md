# Issue

**Title:** Fix write_pdb to always define open

## Description

Since open is defined as a local variable (if defer_writing) it won't be looked up in the global namespace (if not defer_writing).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `16de64e3f391`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.