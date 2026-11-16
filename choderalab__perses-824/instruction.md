# Issue

**Title:** Fix #811: Restore default timesteps to 4 fs, constrained bonds to hydrogen

## Description

: Restores default timesteps to 4 fs, constrained bonds to hydrogen

Note that the pathological constraint issue was fixed in openMM 7.5.0

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db4fe97f6734`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.