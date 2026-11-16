# Issue

**Title:** Synchronize the changes in tracked LAMMPS files and small bugfix

## Description

Several formatting changes and small fixes were added in the LAMMPS repo in the last months, leading to divergent history from their copies in this repo.

There was also a small bug in how MPI features are handled when a MPI library is unavailable. (Colvars considers MPI absent, but LAMMPS has a stubs library that defines its on `MPI_Comm` type).

Both changes address errors in building LAMMPS and running its tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6cf0b4467f11`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.