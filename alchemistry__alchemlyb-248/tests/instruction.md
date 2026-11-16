# Issue

**Title:** Fix dhdl2series and u_nk2series would not reattach the unit

## Description

Fix the issue that dhdl2series and u_nk2series would not reattach the unit.
Add a test to ensure all preprocess function would retain the units.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b9497852937a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.