# Issue

**Title:** Fix 1-4 scaling factors for both ligand and protein.

## Description

This PR addresses a long standing gotcha with our 1-4 scaling factors for electrostatics for both proteins and ligands. This PR implements them correctly (net effect is strengthening the 1-4 electrostatic interactions from 0.5 to 0.833).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eda5a0d0bd65`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.