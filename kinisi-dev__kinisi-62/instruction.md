# Issue

**Title:** Replace the _get_matrix function.

## Description

The _get_matrix function may cause errors when working with triclinic cells. It has been replaced with .triclinic_dimensions

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ef30e93d0c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.