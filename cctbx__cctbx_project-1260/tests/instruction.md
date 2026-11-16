# Issue

**Title:** simtbx/kokkos: build with shared cudart

## Description

simtbx's Kokkos library was linked against nvcc's default static CUDA runtime, while libkokkoscore.so links the shared libcudart.so, so the process held two independent CUDA runtimes. With Kokkos 4.7 / CUDA 13 this fails on the first kernel launch with cudaErrorInvalidResourceHandle from cudaFuncGetAttributes. Therefore, switch to linking against only the shared runtime.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9930e10fc0d1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.