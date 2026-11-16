# Issue

**Title:** Adds missing `ENDMDL` on `HETATM` in `pdb_tidy`

## Description

Solves #160

* Add case for `ENDMDL` when last atom line is `HETATM`
* Add tests
* Add new example from #160 

All previous tests are passing on my machine.

Well noticed, @amjjbonvin

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `34a32b6dcaec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.