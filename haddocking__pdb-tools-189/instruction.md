# Issue

**Title:** [PATCH] Error on residue names longer than 3 characters in pdb_fromcif

## Description

PDB residue names are three characters. `pdb_fromcif` was writing the whole 5-character CCD code (A1AJ1, etc.) into that field, so the chain id and residue number moved two columns to the right. no message on stderr.

the script already bails on a chain id longer than one character and on residue numbers over 9999. i added the same check for residue names.

test feeds a tiny cif with `A1AJ1` and expects exit code 1.

whitespace-split readers still saw the letters, but anything that trusts the column layout (pretty much every old PDB parser) did not. this keeps the file legal or refuses to write it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `19d55fb7a13a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.