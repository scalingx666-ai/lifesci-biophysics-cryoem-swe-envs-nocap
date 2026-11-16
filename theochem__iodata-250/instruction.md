# Issue

**Title:** Add support for `CONECT` lines in PDB file (load and dump bonds)

## Description

This adds support for the `CONECT` lines in pdb load and dump functions. I also split up the load_one functions over a few helpers to stop pylint from complaining about too long functions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4a9b322cc4a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.