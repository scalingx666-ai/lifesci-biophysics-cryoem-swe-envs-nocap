# Issue

**Title:** Add parser for gmxMMPBSA residue decomposition dat files

## Description

## Summary
- add `parse_gmxMMPBSA_decomp_dat` to load gmxMMPBSA per-residue decomposition `.dat` outputs
- cover parsing of Generalized Born and Poisson Boltzmann sections including total, sidechain and backbone tables
- test dat parser using synthetic GB and PB blocks
- parse averaged `.dat` decomposition files in `start` to aggregate GBSA/PBSA residue contributions

## Testing
- `pytest streamd/tests/decomp_dat_parser_test.py -q`
- `pytest streamd/tests/decomp_parse_test.py streamd/tests/results_parse_test.py -q`

------
https://chatgpt.com/codex/tasks/task_e_68a5e5835ddc832bb293f80979ed613d

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ef6aa1065c4f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.