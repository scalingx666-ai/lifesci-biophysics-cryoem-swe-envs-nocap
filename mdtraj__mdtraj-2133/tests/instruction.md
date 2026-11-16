# Issue

**Title:** Implement RMSF for residues

## Description

Hi - I've added code to _rmsf.pyx to implement this, and added a test. The approach matches that used by GROMACS - residue RMSFs are calculated from mass-weighted constituent atom RMSFs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `863d5c51d325`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.