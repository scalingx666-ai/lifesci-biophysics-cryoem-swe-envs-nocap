# Issue

**Title:** Writting options depending on the size of the system

## Description

For small systems, write config.pdb and config.mmcif.
For systems too big for PDB files, alert the user and write only config.mmcif.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c81680818f1e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.