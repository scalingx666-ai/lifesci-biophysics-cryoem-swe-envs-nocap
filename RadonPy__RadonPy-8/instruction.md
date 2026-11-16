# Issue

**Title:** Release/0.2.2

## Description

- Psi4 version 1.6 is now supported.
- Implemented potential energy surface scan by Psi4.
- Implemented constrained geometry optimization by Psi4.
- Added CCSD method for polarizability calculation to `psi4_wrapper.Psi4w.cc2_polar`.
- Fixed a bug that caused failure of geometry optimization in Psi4 for certain molecules due to failure of inverse transformation of internal coordinates.
- Fixed return values when `poly.polyinfo_classifier` caused an error.
- Fixed a bug that caused failure of 3D coordinates generation for certain molecules in `utils.mol_from_smiles`.
- Fixed a bug that caused an error in `poly.substruct_match_smiles_list`.
- Improved availability checks for LAMMPS packages.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d19245e1d559`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.