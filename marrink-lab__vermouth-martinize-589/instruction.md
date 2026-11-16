# Issue

**Title:** Lazy merge

## Description

Lazy merging of chains as per #587. Instead of specifying groups of chains to merge, users can just give `-merge all` and all chains get merged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7e359bde7397`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.