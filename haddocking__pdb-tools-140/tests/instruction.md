# Issue

**Title:** Correct `pdb_tidy` when used with lists in lib

## Description

`pdb_tidy` partially exhausts the `fhandle` iterator to obtain initial information about the PDB. However, when running `pdb_tidy.run` with lists, this "exhaustion" does not work because the list starts from scratch in the second `for`-loop. This PR corrects for this by making the input always an iterator regardless of its type.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9e9f89d031e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.