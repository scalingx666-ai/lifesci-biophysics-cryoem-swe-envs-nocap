# Issue

**Title:** Fix 'hbond' command

## Description

Action_Hbond has been renamed Action_HydrogenBond; related to Amber-MD/cpptraj#485.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6599ab363d0b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.