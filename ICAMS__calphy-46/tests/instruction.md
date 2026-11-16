# Issue

**Title:** Allow  custom cmdargs for lammps object

## Description

Allows to add a cmdargs str to the MD block that is passed to the lammps object. This in turn allows to use accelerator packages for lammps, f.e. -sf intel or -sf kokkos

Also attempts to fix issues with potentials like MTP which only use "* *" as pair_coeff str by adding it directly to the list. The prrevious implementation tried to pass the str '["*", "*"]' to lammps  which obviously does not work.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `909ab5147c22`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.