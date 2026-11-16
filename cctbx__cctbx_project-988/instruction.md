# Issue

**Title:** Undowser2

## Description

The major change is a modification to probe2 and the mmtbx.probe libraries so that they avoid collisions between atoms in incompatible conformations.

This problem was found while adding a new undowser2_validation.py script that uses the new probe and reduce to do its thing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a90a4f3bc54e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.