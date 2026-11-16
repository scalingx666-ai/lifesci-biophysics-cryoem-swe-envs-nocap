# Issue

**Title:** Release 0.22.0

## Description

This PR releases the 0.22 release which includes the RMSD restraint which requires OpenMM 7.3 from the dev branch for now, but does not require OpenMM 7.3 to install.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0860433636f1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.