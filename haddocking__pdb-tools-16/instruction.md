# Issue

**Title:** Added pdb_delinsertions to (selectively) delete insertion codes.

## Description

Following issue #13, here is a pull request to try and address that need. `pdb_delinsertion` removes, by default, _all_ insertions and pads the numbering accordingly. It also allows the user to specify insertions that should be removed, ignoring all others (e.g. `pdb_delinsertion -A99,B24` removes only insertions at residues 99 of chain A and 24 of chain B).

Comments? @mtrellet @joaomcteixeira @amjjbonvin

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db933d99ba10`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.