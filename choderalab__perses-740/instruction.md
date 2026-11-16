# Issue

**Title:** Deterministic Geometry Proposal

## Description

## Description
allow for deterministic placement of `unique_new` atoms by bypassing stochastic placemement in `geometry.py`

## TODO:
- [x] implement changes to `RelativeFEPSetup`
- [x] write test
- [x] assert `core_atoms` are unchanged in position

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a81418e6bf7b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.