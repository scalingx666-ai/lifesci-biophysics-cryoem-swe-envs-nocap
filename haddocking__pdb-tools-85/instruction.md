# Issue

**Title:** Fix count of HETATM to include all HETATM atoms, not residues.

## Description

`pdb_wc` was reporting the number of unique HETATM residues, not number of atoms.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `829e88bc39c8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.