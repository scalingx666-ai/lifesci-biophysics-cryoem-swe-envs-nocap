# Issue

**Title:** FIx module path

## Description

A simple bugfix for a function that was incorrectly imported from `openmmtools.utils` instead of `openmmtools.multistate.utils`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b31737811a5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.