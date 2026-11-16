# Issue

**Title:** Formal Charge <NA> from Mol2 Files

## Description

#2087 

- When generating Atoms in topology, set formal_charge as None instead of the pandas `<NA>` sentinel value if it is `<NA>`.
- Added test for writing mol2 files as pdb and rereading it again.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8edbc7330311`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.