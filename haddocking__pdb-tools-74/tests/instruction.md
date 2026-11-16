# Issue

**Title:** Ignores CONECT lines in pdb_tidy

## Description

After the discussion in #72, `pdb_tidy` now ignores `CONECT` lines, removing them from the output.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `36208c946b05`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.