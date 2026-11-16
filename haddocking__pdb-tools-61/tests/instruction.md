# Issue

**Title:** Reset residue numbering for each model

## Description

Since large ensembles tend to surpass 9999 overall residues, I tweaked `pdb_reres` so that the numbering scheme will reset between models. Since `pdb_reatom` already does this for each model, this would also help improve consistency between scripts.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `061e1b74b6b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.