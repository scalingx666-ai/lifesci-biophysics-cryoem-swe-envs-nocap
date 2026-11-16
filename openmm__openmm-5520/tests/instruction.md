# Issue

**Title:** Reduce energy parameter derivatives on the device

## Description

## Summary

This PR introduces a simple change that moves the reduction of context-parameter derivatives from the host to the device on GPU platforms.

---

**AI disclosure.** The code change and the benchmark script were produced with Claude Code using Claude Opus 5.5. I reviewed the change and verified the results reported here.

## Problem

On the common platforms (CUDA, OpenCL, HIP), `CommonUpdateStateDataKernel::getEnergyParameterDerivatives` downloads the whole per-thread derivative buffer and sums it on the host. The buffer has `numDerivs*numEnergyBuffers` entries, where `numEnergyBuffers = max(numThreadBlocks*ThreadBlockSize, ...)`. That is about 49,000 values per parameter on an RTX 4090, independent of the system size. Each query therefore costs about 0.5 ms more than the energy alone, plus about 0.1 ms for every additional parameter. For small and medium systems, this is several times the cost of the energy evaluation itself.

This cost is paid by:

- every `getState(getParameterDerivatives=True)`;
- every step of a `CustomIntegrator` that uses `deriv(energy, param)`;
- every evaluation of a `CustomCVForce` whose inner forces request derivatives, because it queries the inner context. This happens even when only the energy of the outer context is requested.

## Change

A `reduceEnergyParamDerivs` kernel in `utilities.cc`, modeled on `reduceEnergy`, sums the buffer on the device. Each work group handles one parameter, so only `numDerivs` values are transferred. `ComputeContext::reduceEnergyParamDerivs` launches it, and `CommonUpdateStateDataKernel::getEnergyParameterDerivatives` calls it instead of reducing on the host. The buffer layout (`energyParamDerivs[GLOBAL_ID*numDerivs+index]`) and every kernel that writes to it are unchanged.

4 files, +58/−21, all in `platforms/common`.

## Benchmarks

### Benchmark system

| | |
|---|---|
| GPU | NVIDIA GeForce RTX 4090, 24 GB, PCIe 4.0 x16 (one of two in the machine) |
| NVIDIA driver | 595.71.05 (OpenCL 1.2 CUDA through the NVIDIA ICD) |
| CPU | AMD Ryzen Threadripper PRO 5975WX, 32 cores / 64 threads |
| Memory | 503 GB |
| OS | Ubuntu 22.04.5 LTS, Linux 5.15.0-185-generic |
| Compilers | gcc/g++ 11.4.0, nvcc 12.9.86 (CUDA toolkit 12.9 from conda-forge), CMake 4.4.4 |
| Python | 3.13.16, NumPy 2.5.4 |
| OpenMM | master at `3a2326d0d`, `CMAKE_BUILD_TYPE=Release` (`-O3 -DNDEBUG`), CUDA and OpenCL platforms |

Both builds use the same configuration and differ only in this commit. The benchmarks ran on the second GPU, which was otherwise idle; the other GPU was running an unrelated job throughout. All runs use mixed precision. The two builds were run one after the other with the same `libOpenMM.so` and Python package, switching only `OPENMM_PLUGIN_DIR`; the change is confined to the platform libraries. Times are in ms, averaged over 500 calls after one warm-up call. The script is [bench_param_derivs.py](https://github.com/user-attachments/files/33747955/bench_param_derivs.py).

**`getState(getEnergy=True, getParameterDerivatives=True)`**, master → this PR (energy alone for comparison):

| Atoms | Energy only | 1 parameter | 2 parameters | 4 parameters | 8 parameters |
|---|---|---|---|---|---|
| CUDA, 1,503 | 0.078 → 0.070 | 0.565 → 0.114 | 0.654 → 0.115 | 0.800 → 0.132 | 1.281 → 0.142 |
| CUDA, 6,282 | 0.117 → 0.108 | 0.609 → 0.154 | 0.691 → 0.153 | 0.924 → 0.171 | 1.302 → 0.179 |
| CUDA, 50,442 | 0.343 → 0.343 | 0.824 → 0.376 | 0.902 → 0.384 | 1.074 → 0.406 | 1.526 → 0.420 |
| OpenCL, 1,503 | 0.109 → 0.107 | 0.590 → 0.155 | 0.695 → 0.158 | 0.845 → 0.174 | 1.242 → 0.174 |
| OpenCL, 6,282 | 0.162 → 0.164 | 0.694 → 0.205 | 0.845 → 0.207 | 0.993 → 0.223 | 1.388 → 0.229 |
| OpenCL, 50,442 | 0.436 → 0.435 | 0.926 → 0.460 | 1.050 → 0.461 | 1.251 → 0.491 | 1.630 → 0.490 |

**`CustomIntegrator` step with `deriv(energy, p_i)` for every parameter** (velocity Verlet with constraints), master → this PR:

| Atoms | No `deriv` | 1 parameter | 8 parameters |
|---|---|---|---|
| CUDA, 1,503 | 0.055 → 0.051 | 0.568 → 0.121 | 1.301 → 0.145 |
| CUDA, 6,282 | 0.140 → 0.142 | 0.654 → 0.204 | 1.330 → 0.229 |
| CUDA, 50,442 | 0.254 → 0.256 | 0.783 → 0.341 | 1.460 → 0.370 |
| OpenCL, 1,503 | 0.090 → 0.089 | 0.608 → 0.169 | 1.270 → 0.187 |
| OpenCL, 6,282 | 0.170 → 0.174 | 0.797 → 0.251 | 1.410 → 0.271 |
| OpenCL, 50,442 | 0.315 → 0.316 | 0.858 → 0.423 | 1.525 → 0.457 |

**`getState(getEnergy=True)` with a `CustomCVForce` whose inner force requests one derivative** (no other derivatives in the system), master → this PR:

| Atoms | CUDA | OpenCL |
|---|---|---|
| 1,503 | 0.603 → 0.145 | 0.657 → 0.218 |
| 6,282 | 0.627 → 0.184 | 0.731 → 0.270 |
| 50,442 | 0.871 → 0.440 | 0.947 → 0.530 |

Steps that don't compute derivatives are unaffected: a `LangevinMiddleIntegrator` step takes 0.051 → 0.047 ms (1,503 atoms) and 0.240 → 0.241 ms (50,442 atoms) on CUDA.

**Standard benchmark suite** (`examples/benchmarks/benchmark.py --precision mixed --seconds 30`), ns/day, master → this PR. None of these tests use parameter derivatives, so this checks for regressions:

| Test | CUDA | OpenCL |
|---|---|---|
| pme | 2002.6 → 2000.2 | 1612.0 → 1604.7 |
| apoa1pme | 740.8 → 740.2 | 559.3 → 565.0 |
| amber20-dhfr | 2039.9 → 2038.6 | 1709.5 → 1702.5 |

All differences are within run-to-run noise.

## Correctness

- The derivative values printed by the benchmark script are identical between the two builds, to all printed digits, on both platforms and at every system size.
- Against the Reference platform, in a 4,000-particle system with 0, 1, 2, 3, 7, and 14 derivative parameters spread over a `CustomNonbondedForce`, a `CustomBondForce`, and a `CustomCVForce` with its own inner derivative:
  - double precision: maximum relative error ≤ 1e-14 on CUDA and OpenCL (about 4e-14 with the host-side sum in OpenMM 8.6.1);
  - single and mixed precision: 1.5e-4, identical to the unmodified code.
- Tests that exercise parameter derivatives all pass on CUDA and OpenCL in single, mixed, and double precision: `CustomBondForce`, `CustomAngleForce`, `CustomTorsionForce`, `CustomNonbondedForce`, `CustomCompoundBondForce`, `CustomCentroidBondForce`, `CustomGBForce`, `CustomCVForce`, and `ATMForce`.
- Full CUDA and OpenCL suites (`ctest -R "^Test(Cuda|OpenCL)"`): 389 of 392 passed. The three failures also occur on unmodified master with the same toolchain:
  - `TestCudaNonbondedForceMixed` and `TestOpenCLNonbondedForceMixed`, at `testReordering` (`TestNonbondedForce.h:1242`), on every run;
  - `TestOpenCLMonteCarloFlexibleBarostatMixed`, which is stochastic and passes and fails intermittently on master.
- HIP was not tested (no AMD GPU available). The change only uses `ComputeContext`/`ComputeKernel` APIs that HIP shares with CUDA and OpenCL.

## Reproducing the benchmark

```bash
# Same Python environment, switching only the plugin directory between the two builds
OPENMM_PLUGIN_DIR=<build>/lib/plugins python bench_param_derivs.py --platform CUDA --precision mixed
OPENMM_PLUGIN_DIR=<build>/lib/plugins python bench_param_derivs.py --platform OpenCL --precision mixed
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `308e446242e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.