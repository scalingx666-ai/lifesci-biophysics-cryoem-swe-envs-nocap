# Issue

**Title:** Direct integration of common docking output files

## Description

Follow-up on issue #11 
- Adds `prolif.Molecule` suppliers for common docking output files (PDBQT, SDF, MOL2)
- Adds a new method (`Fingerprint.run_from_iterable` ) that computes a fingerprint using one of those suppliers

- [x] Documentation
- [x] Notebooks updated
- [x] Tests

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `67d5d69c8614`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.