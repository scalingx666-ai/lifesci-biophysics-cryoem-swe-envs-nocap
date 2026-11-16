# Issue

**Title:** Exchanging from hand-made duplication View to ScatterView

## Description

Description of changes:
- The variables local_force, local_torque and local_virial implemented by Kokkos::View is exchanged to ones implemented by Kokkos::ScatterView

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1c3f9e31e45f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.