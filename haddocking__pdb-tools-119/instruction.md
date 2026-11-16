# Issue

**Title:** Correct `MODEL`, `END` and `ENDMDL` in `pdb_tidy`

## Description

And also,
* Adds `ENDMDL` when missing
* Corrects error with duplicated `END` insertion.
* all previous tests pass, add additional tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2fecdebd7520`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.