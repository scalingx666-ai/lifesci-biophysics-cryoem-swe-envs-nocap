# Issue

**Title:** fix block name HSD

## Description

residue HSD had the BB resname HIS. This should change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6a0cc604b27d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.