# Issue

**Title:** Molecular dynamics: a fix of one's own in the pieces of a run, and an example

## Description

- **Fix.** `MolecularDynamics.run` started with `run 0` and went on in pieces with `pre no`, so fixes that plan the whole run at their setup planned one of 0 steps: a `fix deform` added with `command` stretched nothing. The first `run 0` now has `start` and `stop`; a new test compares a run with `fix deform` with the same run written as LAMMPS input (atol 1e-9) and fails without the fix. All tests of simulation and lattice dynamics pass (261).
- **Example**, Molecular dynamics through LAMMPS (`docs/examples/molecular-dynamics.md`), with the potential of Zhou written by MDAPY, so it needs only LAMMPS:
  - heating copper from 300 to 2200 K, the solid fraction from the bonds of q6 of the frames: it melts at about 1550 K, superheated, with a pointer to `melting_point`;
  - stretching along x with `fix deform` at 1 K and 300 K: the 1 K run follows the static stress of the potential up to some 7 %;
  - what the interface does, what it is not made for (charges and molecules, other units, MPI, integrators of one's own, many frames) and what to do instead;
  - Kokkos on threads and on a GPU: `cmdargs`, the log that shows it, one initialization per process.
- Pictures made locally with LAMMPS stable_30Sep2026 (the blocks are `skip-test`).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2c45824924a6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.