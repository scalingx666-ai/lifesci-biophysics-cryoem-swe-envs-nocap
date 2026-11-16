# Issue

**Title:** renamed pdb_delinsertion

## Description

`pdb_delinsertion` was renamed to `pdb_renuminsertion`.

 

:warning: @JoaoRodrigues @amjjbonvin 
This change should upgrade `pdb-tools` to version 3 by definition of SV2. This PR breaks backward compatibility.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3dbfcbb2e90f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.