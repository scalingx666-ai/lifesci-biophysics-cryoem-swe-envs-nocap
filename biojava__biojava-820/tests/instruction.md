# Issue

**Title:** Improved ASA calculation performance

## Description

Now neighbors are calculated with spatial hashing, improving performance dramatically for cases with many thousands of atoms. Also improved docs, logs and added new tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f9bfd939e720`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.