# Issue

**Title:** Kokkos support for dipolar direct summation

## Description

- **Add design spec for Kokkos+SIMD dipolar direct sum port**
- **Add MPI latency-hiding to energy kernel in DDS Kokkos spec**
- **Add implementation plan for Kokkos+SIMD dipolar direct sum**
- **Correct DDS plan to use non-deprecated Kokkos simd<double>**
- **magnetostatics: add templated dipolar pair kernels (scalar + simd)**
- **magnetostatics: add SoA repack and image-shift helpers for DDS**
- **magnetostatics: port DDS forces to Kokkos + kokkos-simd**
- **magnetostatics: port DDS energy to Kokkos + kokkos-simd**
- **magnetostatics: port DDS dipole-field to Kokkos, drop scalar kernels**
- **magnetostatics: post-review cleanup of DDS Kokkos port**
- **testsuite: make dipolar DDS inner-loop consistency test rank-count robust**
- **magnetostatics: vectorize DDS min-image and load-balance triangular loops**
- **magnetostatics: drop kokkos-simd from DDS, keep Kokkos threading**
- **magnetostatics: drop SoA repack from DDS, read gathered data directly**
- **magnetostatics: post-review fixes for DDS Kokkos port**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4da8c4828587`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.