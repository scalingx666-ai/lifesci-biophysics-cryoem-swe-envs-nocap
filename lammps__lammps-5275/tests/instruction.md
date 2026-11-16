# Issue

**Title:** Optimize PairSW and PairSWOMP three-body loops by caching per-neighbor radial terms

## Description

**Summary**

Optimizes the three-body loops of `PairSW` (`pair_style sw`) and `PairSWOMP` (`pair_style sw/omp`) by precomputing per-neighbor distance, reciprocal, and radial exponential terms once per short-range neighbor during the two-body pass instead of recomputing `sqrt()` and `exp()` inside every `(j, k)` triplet iteration.

On 32k- and 64k-atom crystalline silicon benchmarks (`Si.sw`), this cuts `Pair` time by **43.4%-47.8%** (**37.4%-46.6%** less total loop time, **1.60x-1.87x** speedup) with byte-for-bit identical thermo and per-atom trajectories on `sw`. The only extra memory is a small per-neighbor scratch array (72 bytes per short-range neighbor, under 1 KB per MPI rank or OpenMP thread for crystalline Si).

**Related Issue(s)**

None

**Author(s)**

Yoel Sevi, Google DeepMind (ysevi@google.com)
Follow-up changes for sw/mod, sw/angle/table, and KOKKOS: Axel Kohlmeyer, Temple U

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

The code changes were generated using Google Gemini. This includes profiling the Stillinger-Weber compute kernels, drafting the per-neighbor `ShortLeg` caching in `PairSW` and `PairSWOMP`, and running the numerical and performance verification scripts. The human author directed the work, reviewed the resulting code, stripped marginal hunks, and validated correctness and benchmark results.

The follow-up changes for sw/mod, sw/angle/table, sw/kk, and sw/mod/kk (commits cd50410c7d and 44e94ae518) were made and verified with Claude Code (Opus 5.5).

**Backward Compatibility**

Fully preserved. No changes to input script syntax, potential file formats, restart data, or public APIs.

**Implementation Notes**

*Mechanism (5 files, +116 / -95 lines).*
- In unmodified `PairSW::compute()` and `PairSWOMP::eval()`, each central atom `i` has `M = numshort` neighbors within the cutoff (`M = 4` in diamond Si, higher in disordered/liquid phases). The inner `O(M^2)` triplet loop previously called `PairSW::threebody()` for all `M(M - 1)/2` pairs, recomputing `sqrt(rsq1)`, `sqrt(rsq2)`, `1.0 / (r - cut)`, and `exp(sigma_gamma / (r - cut))` on every pair.
- `PairSW` adds a compact `ShortLeg` struct populated via `fill_shortleg()` during the existing two-body neighbor pass (`x[j] - x[i]` displacement signs match stock `delr1`/`delr2`, preserving IEEE-754 `+0.0` sign bits on lattice-aligned neighbors). The inner loop evaluates `threebody_cached()` using products and additions plus one scalar reciprocal `1.0 / (s1.r * s2.r)` per triplet.
- `PairSW::threebody()` now delegates to `fill_shortleg()` and `threebody_cached()` so the three-body physics equations remain in one place (`-48` duplicate lines).
- Subclasses that override `virtual threebody()` (`PairSWMOD` and `PairSWMODOMP`) set `sw_mod_flag = 1` in their constructors (`0` in `PairSW`). When `sw_mod_flag` is set, `fill_shortleg()` records only the geometry (`del`, `rsq`, `j`, `jtype`), skipping unused `sqrt()`/`exp()` calls, and `compute()`/`eval()` dispatch through virtual `threebody()`.
- When `pair_style sw threebody off` is active (`skip_threebody_flag`), `shortleg` population and resizing are skipped entirely. `neighshort` allocation in `PairSW::allocate()` is retained because `PairSWAngleTable` overrides `compute()` and uses `neighshort`, while the unused `neighshort` writes in `PairSW::compute()` and `PairSWOMP::eval()` are removed.

*Benchmarks.* Median of 5 interleaved A/B trials (3 trials for `sw/mod`) after warmup on pinned physical cores of an AMD EPYC 7B13 (`g++ 15.3.0 -O3 -march=native -fopenmp -DNDEBUG`, MPICH 5.0.2), comparing `develop` (`5f0288a471`) against this branch:

| Workload (layout) | Loop time (base -> opt) | Loop time saved | Pair time saved | Throughput (katom-step/s) |
|---|---|---|---|---|
| 64k Si, 1000 steps (12 MPI) | 7.392 s -> 4.484 s | **39.3%** (1.65x) | **43.4%** (1.77x) | 8659 -> 14273 |
| 64k Si, 1000 steps (6 MPI x 2 OMP) | 7.625 s -> 4.412 s | **42.1%** (1.73x) | **46.4%** (1.87x) | 8393 -> 14505 |
| bench/POTENTIALS/in.sw, 32k (12 MPI) | 0.362 s -> 0.210 s | **41.8%** (1.72x) | **44.4%** (1.80x) | 8851 -> 15218 |
| bench/POTENTIALS/in.sw, 32k (6 MPI x 2 OMP) | 0.361 s -> 0.226 s | **37.4%** (1.60x) | **44.1%** (1.79x) | 8867 -> 14173 |
| bench/POTENTIALS/in.sw, 32k (1 MPI) | 3.779 s -> 2.136 s | **43.5%** (1.77x) | **44.6%** (1.81x) | 847 -> 1498 |
| bench/POTENTIALS/in.sw, 32k (1 MPI x 1 OMP) | 3.807 s -> 2.033 s | **46.6%** (1.87x) | **47.8%** (1.92x) | 841 -> 1574 |
| sw/mod 32k Si, 100 steps (12 MPI) | 0.508 s -> 0.480 s | **5.4%** (1.06x) | **10.3%** (1.12x) | 6304 -> 6665 |
| sw/mod 32k Si, 100 steps (6 MPI x 2 OMP) | 0.495 s -> 0.460 s | **7.2%** (1.08x) | **5.2%** (1.06x) | 6462 -> 6963 |

The `sw/mod` rows are a non-regression check from 3 short trials and are noisier than the others.

*Correctness and build verification.*
- `ctest -R sw`: `19/19` tests passed (all `.plain`, `.omp`, `.eatom_only`, `.vatom_only`, `.extract`, and `.single` test cases active and executed).
- Exact trajectory checks (`MALLOC_PERTURB_=165`): `18/18` 50-step runs produced byte-for-bit identical `%.16g` thermo output every step and byte-for-bit identical `%.17g` sorted per-atom dumps (`x, y, z, fx, fy, fz, pe/atom, stress/atom`) across `sw` (triclinic strained Si, 2-element `CdTe.sw`, and 3-element `CdTeZnSeHgS0.sw` at 1 and 4 MPI ranks), `sw threebody off` (`1x1`, `4x1`, `1x2` OMP, `2x2` OMP), `sw/mod maxdelcs 0.20 0.40` (`1x1`, `4x1`, `1x2` OMP, `2x2` OMP), `hybrid/overlay lj/cut sw` (`1x1`, `4x1`), and `sw/angle/table` (`1x1`, `4x1`).
- OpenMP precision (`2 MPI x 2 OMP`): `sw/omp` matches unmodified `sw/omp` to `0.0` relative potential energy and pressure difference at steps 0 and 50, and `2.07e-16` (`<= 1` ULP) max per-atom force difference at step 0.
- Finite-difference forces (`fix numdiff` on strained triclinic Si): analytic vs. finite-difference max force discrepancy matches unmodified `develop` (`1.218e-7` on `sw`, `4.682e-8` on `sw/omp`).
- Style & build systems: verified with both CMake and conventional GNU Make (`make yes-manybody yes-openmp && make serial mpi`), `-Wall -Wextra -Werror -Werror=vla` under C++17, C++23, and `-DLAMMPS_BIGBIG`, `clang-format --style=file --dry-run --Werror`, and `make check-whitespace check-permissions check-homepage check-errordocs check-fmtlib`.

*Follow-up changes (commits cd50410c7d and 44e94ae518).* They replace the fallback of `sw/mod` to the virtual `threebody()` and the retained `neighshort` list described above, and apply the same idea to the KOKKOS versions:
- `sw/mod` and `sw/mod/omp` also use the leg cache.  `delta1`/`delta2` move into `PairSW` and `threebody_cached()` becomes a template, whose `MODFLAG=1` variant applies the sw/mod taper; `compute()`/`eval()` select the variant once per call.  A run-time check of `sw_mod_flag` inside the inlined triplet function instead slowed down plain `sw` by up to 20% with OpenMP threads.  The duplicated `threebody()` overrides of `sw/mod` and `sw/mod/omp` are removed.
- `sw/angle/table` fills the same cache and uses a new `threebody_table()` that takes two cached legs.  With that, `neighshort`, `PairSW::threebody()`, and `ShortLeg::rsq` are no longer used and are removed (64 bytes per cache entry).
- `sw/kk` and `sw/mod/kk` recomputed `sqrt()` and `exp()` of both legs for every (j,k) pair.  They now compute the factors of the i-j leg once per neighbor j, outside of the k loop (`threebody_leg()`), and `threebody_kk()` combines two legs.  No extra storage is needed.  Neither GCC nor nvcc moves these computations out of the loop by themselves.

*Additional verification* (AMD Ryzen 7 7840HS with GCC 15 and MPICH; one NVIDIA V100 with CUDA 12.9 and KOKKOS in double precision):
- Thermo output and final forces (`%22.16g`) are bitwise identical to `develop` in 22 cases: `sw` for crystalline Si, liquid Si at 2000 K, mW water at 298 K (64k atoms each), and `CdTeZnSeHgS0.sw`; `sw threebody off`; `sw/mod` for a MoS2 layer with and without `maxdelcs` (the chosen values exercise the taper); `sw/angle/table` (CG SPC/E water in `hybrid/overlay`); each with several MPI and OpenMP layouts, and `sw/kk` and `sw/mod/kk` with the KOKKOS OpenMP back end.  The liquids have up to 17 neighbors within the cutoff, which exercises the growth of the `shortleg` array; valgrind memcheck reports no errors for sw, sw/omp, sw/mod, sw/mod/omp, and sw/angle/table.
- The unit tests of the sw styles pass, including the KOKKOS OpenMP and (on the V100) KOKKOS CUDA variants.  On the V100 the KOKKOS results agree with `develop` within the run-to-run noise.

| Workload (MPI x OpenMP threads) | develop | this PR | speedup |
|---|---|---|---|
| sw, liquid Si 2000 K, 64k atoms (1 x 1), Pair time | 11.60 s | 5.35 s | 2.17x |
| sw, liquid Si 2000 K, 64k atoms (8 x 1), Pair time | 4.55 s | 2.15 s | 2.12x |
| sw, mW water 298 K, 64k atoms (1 x 1), Pair time | 10.38 s | 5.12 s | 2.03x |
| sw, mW water 298 K, 64k atoms (8 x 1), Pair time | 4.07 s | 2.03 s | 2.00x |
| sw/mod, MoS2 12k atoms (1 x 1), Pair time | 1.876 s | 1.179 s | 1.59x |
| sw/mod/omp, MoS2 12k atoms (1 x 8), Pair time | 1.672 s | 1.061 s | 1.58x |
| sw/angle/table, CG SPC/E water 64k (1 x 1), Pair time | 8.69 s | 7.96 s | 1.09x |
| sw/kk, Si crystal 64k (KOKKOS OpenMP, 1 x 8), Loop time | 3.84 s | 3.51 s | 1.09x |
| sw/kk, liquid Si 64k (KOKKOS OpenMP, 1 x 8), Loop time | 4.67 s | 4.23 s | 1.10x |
| sw/kk, mW water 64k (KOKKOS OpenMP, 1 x 8), Loop time | 4.13 s | 3.83 s | 1.08x |
| sw/mod/kk, MoS2 12k (KOKKOS OpenMP, 1 x 8), Loop time | 1.820 s | 1.539 s | 1.18x |
| sw/mod/kk, MoS2 12k (KOKKOS OpenMP, 8 x 1), Loop time | 1.629 s | 1.405 s | 1.16x |
| sw/kk, Si crystal 512k (V100), Loop time | 0.522 s | 0.477 s | 1.09x |
| sw/kk, liquid Si 512k (V100), Loop time | 0.517 s | 0.480 s | 1.08x |
| sw/kk, mW water 512k (V100), Loop time | 0.485 s | 0.445 s | 1.09x |
| sw/mod/kk, MoS2 442k (V100), Loop time | 1.008 s | 0.981 s | 1.03x |

Medians of 3 interleaved runs.  For the KOKKOS styles the Loop time is given, since the Pair time of KOKKOS does not include the time of the GPU kernel, which runs asynchronously.

**Post Submission Checklist**

- [ ] The feature or features in this pull request is complete
- [ ] Licensing information is complete
- [ ] Corresponding author information is complete
- [ ] AI tools attribution is included
- [ ] The source code follows the LAMMPS formatting guidelines
- [ ] The feature has been verified to work with the conventional build system
- [ ] The feature has been verified to work with the CMake based build system

**Further Information, Files, and Links**

Open PR #4911 (centroid stress for many-body styles) edits the same three-body loop in `pair_sw.cpp`, so whichever PR is merged second needs a small rebase; I am happy to do it if this one lands second. Timing summary lines in `examples/` reference logs were left unchanged here and can be updated separately if desired.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f0288a47184`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.