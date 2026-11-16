# Issue

**Title:** Fix error reading CONECT records in PDB with multiple models

## Description

.  This issue affected loading PDB files with PDBFixer, but *not* with PDBFile.  If a file contained `MODEL` and `ENDMDL` records, the `CONECT` records would not get processed correctly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `82703dff7395`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.