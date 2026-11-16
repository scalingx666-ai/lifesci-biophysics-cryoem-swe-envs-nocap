# Issue

**Title:** Simplify testing on python 3.14 (with openmm)

## Description

Simplify testing workflow for python 3.14 when openmm support python 3.14, as per #2089.

To be merged when openmm officially supports python 3.14.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2e88bf69dc38`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.