# Issue

**Title:** Add support for inserting/deleting counterions when changing charge to maintain net neutrality

## Description

- [x] add counterion decoupling in `topology_proposal` for charge-changing mutations
- [x] write test to identify appropriate counterions to decouple
- [ ] update to master

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d215e7a440a6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.