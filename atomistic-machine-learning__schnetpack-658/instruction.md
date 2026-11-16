# Issue

**Title:** fixes bug and updates to new simpy verison

## Description

* New `simpy` version caused problems with `numpy` inputs (`ValueError('expecting integer or half-integer, got 0')`). Just using the `torch` tensors instead solves the issue and prevents an unnecessary conversion from `torch` to `numpy` 

* Fixes a typo

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5dba6119a112`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.