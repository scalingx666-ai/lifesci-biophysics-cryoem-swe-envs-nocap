# Issue

**Title:** Min mode

## Description

Create a minimal script mode which uses LAMMPS executable to run the calculations. Only works for solid, and for direct FE calculation. Therefore it is an experimental mode, and would be reflected in docs later once it is stable.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0a881ce6bd35`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.