# Issue

**Title:** Fix EnergyRefRemover

## Description

Fix the EnergyRefRemover which was accidentally removing the total sum of all atomref energies of the whole batch from the energies

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `94cc325cfab3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.