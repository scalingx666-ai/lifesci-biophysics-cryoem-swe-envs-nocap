# Issue

**Title:** Add `chiangles`

## Description

This computes the standard `chi` angles for standard residues. The table was taken from http://www.mlb.co.jp/linux/science/garlic/doc/commands/dihedrals.html

~~This is best reviewed excluding whitespace differences, as the documentation has a lot of trailing whitespace and my editor is configured to delete it upon file saving. If this is undesirable, I can back this part of the change out.~~ I decided to back it out, as most is trailing whitespace in docs for the column-based (and thus whitespace-sensitive) PDB file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a902b4e35f86`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.