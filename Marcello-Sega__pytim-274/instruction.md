# Issue

**Title:** gitim cleanup & fixes

## Description

* added test with 1 atom in the cell
* added the possibility to change the buffer for PBCs from the interface
  passing buffer_factor=x at init (in units of alpha)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e603166b5064`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.