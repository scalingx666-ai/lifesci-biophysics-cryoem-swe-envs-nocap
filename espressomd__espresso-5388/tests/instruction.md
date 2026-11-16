# Issue

**Title:** Add virial contribution for rigid bonds

## Description

Fixes Partially #5067 

Description of changes:
- Added virial calculation for rigid constraint in pressure.cpp and rattle.cpp
- Added new test script `rigid_bond_virial.py` that verifies the constraint virial

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d0e3986d48e8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.