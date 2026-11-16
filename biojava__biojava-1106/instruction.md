# Issue

**Title:** More on lenient PDBx/mmCIF parsing

## Description

Following #1103 . This covers some fields that are missing in [PDB-IHM](https://pdb-ihm.org/) entries.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `be55381c8b96`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.