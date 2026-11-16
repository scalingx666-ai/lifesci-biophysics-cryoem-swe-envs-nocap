# Issue

**Title:** Remove particle pointer-based access in forces_cabana and force_inline

## Description

Description of changes:
- Added `VelocityViewType = Kokkos::View<double *[3], Kokkos::LayoutRight, Kokkos::HostSpace>`, `DirectorViewType = Kokkos::View<double *[3], Kokkos::LayoutRight, Kokkos::HostSpace>` and `DipmViewType = Kokkos::View<double *, Kokkos::HostSpace>` in aosoa_pack.hpp to avoid accessing the Particle pointer in DPD and dipole interactions
- Added new interface in `gay_berne.hpp`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `76d9e980eaa9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.