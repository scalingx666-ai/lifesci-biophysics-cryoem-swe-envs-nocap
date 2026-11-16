# Issue

**Title:** add secondary structure Information

## Description

Solve the problem in 
Add `ss_code` field to Residue.
Add `run_dssp` and `run_stride`.
Add support for ProteinStructure with multiple models.
Add `ss_code!` to assign ss_code to residue.
Add `ss_code` to read ss_code of residue/atom.
Add `helixselector`, `sheetselector`, `coilselector`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bc70e73e596c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.