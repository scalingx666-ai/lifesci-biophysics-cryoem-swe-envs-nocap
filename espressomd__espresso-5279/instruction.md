# Issue

**Title:** Fix checkpointing of dihedral angles

## Description

Description of changes:
- fix dihedral bond checkpointing bug (regression introduced in 5.0.0)
- re-enable virtual sites relative checkpointing test (regression introduced in 5.0.0)
- improve testing of bonds, particle properties, virtual sites

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e9d1e0dd50e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.