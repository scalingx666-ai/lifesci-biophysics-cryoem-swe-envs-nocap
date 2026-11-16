# Issue

**Title:** Adds tool pdb_chainbows to sequentially assign chain identifiers

## Description

Tool adds chain identifiers sequentially (ABC..abc...012..) based on existing TER records in the file. Addresses the second part of #70.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ffb92cccd26`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.