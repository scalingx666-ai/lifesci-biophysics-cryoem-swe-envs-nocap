# Issue

**Title:** Release v4.1.1

## Description

- match cython types to C ones
- use correct numpy object type
- correct MASK_IDX bitshift in C dataset (fixes limit on number of active datasets)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5a335b6202d8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.