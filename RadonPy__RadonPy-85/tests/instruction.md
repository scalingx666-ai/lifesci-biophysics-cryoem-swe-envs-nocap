# Issue

**Title:** add: Si bond parameters to GAFF2_mod, fix: several bugs and typos

## Description

add: Si parameters for polyalkylsiloxane chain to GAFF2_mod
fix: bug in poly.set_linker_flag, typo in poly.full_match_smiles and poly.substruct_match_smiles

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `83b86c1e57b0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.