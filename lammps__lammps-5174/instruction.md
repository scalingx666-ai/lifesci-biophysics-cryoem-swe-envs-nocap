# Issue

**Title:** KOKKOS: optimize pair_style pace/kk on GPUs and add CPU-backend kernels

## Description

**Summary**

Optimizes the KOKKOS version of `pair_style pace` (ACE) on GPU backends, adds native CPU-backend kernels so that `pace/kk` no longer falls back to the non-accelerated evaluator on host builds.

Measured on the fcc Cu `product` deck, relative to the first commit in this series:

| backend | speedup |
|---|---|
| NVIDIA H100 | **1.76x** |
| AMD MI300A | **1.68x** |
| CPU (single thread, vs. the non-accelerated evaluator) | **~1.7x** |

No new files, no new styles; 12 commits, each one a self-contained change that was benchmarked individually.

**Related Issue(s)**

Addresses lammps/lammps#5063 (`pace/kk` aborting with "Requested too much scratch memory on level 0" on CUDA, or "could not find a valid team size" on HIP).

**Author(s)**

Stan Moore, Sandia National Laboratories

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The code changes were generated using Claude Code (Anthropic). This includes the GPU optimization sequence, the CPU-backend kernels and the dual host/device layout that integrates them, and the accompanying documentation updates. The human author directed the work, selected which changes to keep, reviewed the resulting code, ran all of the hardware benchmarking on H100 and MI300A, and contributed fixes on top of the generated code (including the `grow()` re-allocation fix and the HIP/CUDA compile fixes). Several AI-generated changes were measured, found to be regressions, and dropped before submission (see Implementation Notes).

**Backward Compatibility**

Mostly preserved; three user-visible changes:

- A new optional `neigh auto|shared|global` keyword is added to `pair_style pace` and `pair_style pace/extrapolation`. It is only recognized by the KOKKOS styles; the non-accelerated styles stop with an "unknown keyword" error if it is given. The default `auto` reproduces the previous behavior except that it falls back to level-1 scratch instead of aborting.
- On host backends `pace/kk` now runs the KOKKOS kernels instead of deferring to the non-accelerated evaluator. Results agree with the previous behavior to round-off. The previous restriction that `pace/kk` could only be run on a single CPU thread is lifted.
- The `recursive` evaluator still has no KOKKOS implementation. On host backends it now falls back to the non-accelerated evaluator with a warning instead of being rejected; on GPU backends it remains an error, as before.

No changes are required to existing input scripts.

**Implementation Notes**

*GPU optimizations (commits 1-9).* The largest wins come from changing what is stored rather than how it is computed:

| change | MI300A (incremental) | notes |
|---|---|---|
| store `A_sph`/`weights` as separate re/im arrays | 1.09x | atom index innermost, so the accumulation coalesces |
| de-duplicate the `ComputeDerivative` radial loop | 1.02x | also removes ~60 lines |
| avoid `pow()` for integer exponents | 1.00x | neutral, kept as hygiene |
| fold weights zeroing into `ComputeFS` first-touch | 1.02x | removes a per-chunk `deep_copy` |
| drop the full `A` array, read `A_sph` via conjugate symmetry | 1.00x | removes an array and the `ConjugateAi` pass on device |
| spline writes `fr`/`dfr` directly | 1.05x | drops a transpose copy |
| **eliminate `dB_flatten`** | **1.37x** | largest single win; the leave-one-out products are recomputed in `ComputeWeights` instead of stored |
| **fuse `ComputeRadial` into `ComputeAi`** | 1.03x | **cumulative peak: 1.68x MI300A / 1.76x H100** |

The same ordering holds on both vendors, and the peak is at the same commit on each.

*Shared-memory fallback (commit 10).* `ComputeNeigh` caches the short neighbor list in level-0 (on-chip) team scratch sized `team_size*maxneigh*sizeof(int)`. The maximum available level-0 scratch is now queried from Kokkos via `TeamPolicy::scratch_size_max(0)` rather than assuming a fixed limit, and the kernel transparently falls back to level-1 (global) scratch when the request does not fit, warning once. The `neigh` keyword allows overriding the automatic choice.

*CPU-backend kernels (commits 11-12).* `host_flag` becomes a compile-time `static constexpr`, so the host and device kernel sets are selected with `if constexpr` and only one is ever instantiated (the same pattern as `pair_snap_kokkos`). The host kernels give one thread an entire atom and loop its neighbors inside, which removes every atomic and keeps the atom's accumulators as the innermost working set; the basis-function work is fused (rho -> F(rho) -> weights in one pass), flattened onto precomputed gather offsets, batched across eight atoms, and specialized on the density count at compile time.

Because the two backends want opposite things from the same data, both layouts are kept and only the live one is allocated: the host uses interleaved-complex `A`/`A_sph`/`weights` (re and im in the same cache line for a thread walking one atom), the device uses the split re/im arrays with the atom index innermost (coalescing for a warp scattering across atoms). `fr`/`dfr` are shared.

*Changes that were tried and dropped.* Tiling `ComputeRho`/`ComputeWeights` as an `MDRangePolicy` and adding a `LaunchBounds` occupancy knob were both benchmarked and removed: `LaunchBounds` was neutral to -1% and the `MDRange` conversion cost 16% on MI300A (and ~0.5% on H100). An optional spline-free "direct" Chebyshev radial path and an `O(rank)` prefix/suffix rewrite of the leave-one-out products were also implemented, measured, and dropped (the first for adding ~150 lines for an unmeasurable gain, the second as a large regression caused by the added register pressure).

*Correctness.* Verified against the non-accelerated evaluator on the fcc Cu `product` deck, with both branches of the compile-time host/device switch exercised: per-atom forces agree to 9.8e-13 eV/A and energies to ~13 significant digits. All three `neigh` keyword values give identical forces, an unknown value is rejected, and every commit in the series builds individually.

`kokkos_type.h` gains a one-word fix: `SNAComplex::real_part_product(const complex &)` is now `const`, which resolves an overload-ambiguity warning that GCC emitted at every call site.

**Post Submission Checklist**

- [ ] The feature or features in this pull request is complete
- [ ] Licensing information is complete
- [ ] Corresponding author information is complete
- [ ] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [ ] The feature has been verified to work with the CMake based build system

**Further Information, Files, and Links**

The `neigh` keyword is documented in `doc/src/pair_pace.rst` with a `.. versionadded:: TBD` directive.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `751e53e82884`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.