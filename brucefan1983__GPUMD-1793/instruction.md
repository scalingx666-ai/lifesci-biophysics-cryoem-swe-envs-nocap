# Issue

**Title:** Add analytic NEP Hessian command with dense and sparse output

## Description

## Summary

Add `compute_hessian analytic` to calculate the Cartesian Hessian of the current configuration using a single supported NEP energy model. It executes immediately after being read from `run.in`, must follow `potential`, and does not relax the structure. Ordinary NEP static calculations need no preceding MD run.

Supported models are ordinary NEP energy models, including universal, typewise-cutoff, and flexible ZBL. The implementation uses the shared model reader, independent Hessian parameter storage, and analytic second derivatives. Public changes are limited to command registration and passing existing input/potential information. Ordinary NEP force calculation, neighbor management, and `compute_phonon` remain unchanged.

## Usage and output

Default dense text output (`hessian.out`):

```text
potential nep.txt
compute_hessian analytic
```

Sparse Matrix Market output with diagnostics and a structure snapshot:

```text
potential nep.txt
compute_hessian analytic output_format sparse output hessian.mtx raw_output hessian_raw.mtx metadata hessian.json structure_output hessian.xyz
```

Each command must occupy one line. Both formats support `raw_output`, `metadata`, and `structure_output`. Selected files are overwritten and must use distinct paths that do not alias input files.

The matrix is `H = -dF/dR = d²E/dR²`, in eV/Å², ordered as all x coordinates, then all y coordinates, then all z coordinates. The normal output is `(H + Hᵀ)/2`; raw output retains the unsymmetrized matrix. Neither output is mass weighted, and no acoustic sum rule is imposed. Metadata uses schema version 6.

## Scope and limitations

- Analytic-only, single-GPU ordinary NEP energy models. Temperature-dependent NEP, DFT-D3, multiple potentials, multi-GPU NEP, charge/spin/vdW/ILP models are rejected. There is no finite-difference mode, fallback, or phonon postprocessing.
- Periodic-image contributions for the same atom are summed. Neither format retains lattice-translation labels, so a small-cell matrix cannot directly provide translation-resolved force constants for arbitrary-wavevector phonon dispersion.
- Dense storage scales as `9N²`; sparse storage follows interacting atom blocks. Both remain subject to memory and index limits. Neighbor distances must be at least `1e-6 Å`; `q_123`/`q_233` require `L_max >= 3`, and `q_134` requires `L_max >= 4`.
- Finite matrices that fail symmetry or translation checks can still produce requested raw/metadata files marked `failed_validation`, followed by termination. They are not written as normal results.

## Validation of commit 0cb7a20c

Validated on November 15, 2026, using source commit `0cb7a20cafd4650e11fa71d8d6d128ac341fa8b3`, NVIDIA RTX A2000 Laptop GPU (8 GiB), CUDA 12.8 / nvcc 12.8.93, driver 577.00, Linux/WSL.

- Build passed: `make -C src -j4 gpumd CUDA_ARCH=-arch=sm_86`. The final-commit check confirmed the executable was up to date. Sphinx HTML documentation passed `-W --keep-going`.
- All 12 GPU regression methods passed (184.706 s), plus a separate test of eight invalid invariant/L_max combinations.
- Eleven ordinary-NEP analytic-versus-force-central-difference cases passed using `1e-3 Å` displacement. Maximum relative Frobenius error was `0.000641384536218`, below `1e-3`. The independent reference used ordinary displaced force runs with a negligible MD time step; no FD calculation mode was added to production.
- Each of these 11 cases passed dense/sparse normal and raw matrix comparisons (`atol=1e-9`, `rtol=1e-7`) and metadata diagnostics (`atol=1e-10`, `rtol=1e-7`). Metadata was checked for schema version 6 and absence of `model_temperature_K`.
- Numerical coverage includes `L_max=2..8`, element compaction, radial-only networks, cubic and triclinic small periodic boxes, and ordinary NEP with universal/typewise/flexible ZBL.
- A 15,447-atom sparse calculation passed. Ordinary NEP MD continuation passed after inserting dense and sparse commands in NVE (large box) and NVT (small box), both with ZBL: positions, velocities, forces, energies, and virials matched the uninterrupted baseline within `atol=rtol=1e-10`.
- Sixteen temperature-dependent model rejection cases passed across dense/sparse, large/small boxes, before/after MD, and with/without ZBL. All failed with an explicit unsupported-model message and created none of the requested output files. Temperature-dependent Hessian calculation is unsupported; these are rejection tests only.
- Existing near-coincident-neighbor, DFT-D3/unsupported-command, and input/output collision regressions also passed.

Not verified on this revision: GPU buffers above 16 GiB, full Windows/MSVC/CUDA builds, HIP, or performance benchmarks. Compute Sanitizer was not rerun; the earlier attempt on a previous revision could not initialize the WDDM debugger interface. No passing memory-sanitizer result or historical speedup is claimed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3bf2efdf0f19`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.