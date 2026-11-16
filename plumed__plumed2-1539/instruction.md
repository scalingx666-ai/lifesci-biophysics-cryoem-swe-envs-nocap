# Issue

**Title:** [v2.10] Avoid external BLAS in ActionWithValue::checkForForces

## Description

.

### Problem
Since 8701a7d5ec (v2.10), `ActionWithValue::checkForForces()` accumulates the forces with `plumed_blas_daxpy`. This runs at every step for every action whose value has a force on it. When PLUMED is linked to a multithreaded external BLAS (OpenBLAS, MKL), daxpy goes multithreaded for long vectors (OpenBLAS: n > 10000, i.e. more than ~3300 atoms). These threads come from the BLAS pool, which is sized on the whole node and ignores both `PLUMED_NUM_THREADS` and the number of threads passed by the MD engine (`setNumOMPthreads`), so with GROMACS all the cores get oversubscribed. This is what was reported for `CONTACTMAP ... CMDIST` (a single biased value on thousands of atoms). It worked with v2.9 (plain loop) and gave correct results (only threading is affected).

### Fix
Go back to plain loops (vectorized with `#pragma omp simd`), so that threading is controlled only by PLUMED's own OpenMP settings. The `omp critical` reduction is now entered only when `nt>1`. The `blas/blas.h` include is no longer needed in this file.

### Tests
- Reproduced with a driver run: `CONTACTMAP` with `CMDIST` on 3500 pairs (7000 atoms) + `RESTRAINT`, 400 frames, PLUMED linked to OpenBLAS (pthreads), `PLUMED_NUM_THREADS=1`:
  - before, `OPENBLAS_NUM_THREADS=1`: real ~1.60 s, user ~1.55 s
  - before, `OPENBLAS_NUM_THREADS=2`: real ~1.6-2.0 s, user ~1.9-2.3 s (extra core burned by OpenBLAS)
  - after, `OPENBLAS_NUM_THREADS=2`: real ~1.55 s, user ~1.53 s (no extra threads, not slower)
- `regtest/basic` passes with `PLUMED_NUM_THREADS=1` and `=2` (the latter exercises the `nt>1` branch).

Workaround for users of unpatched versions: `export OPENBLAS_NUM_THREADS=1` (or `MKL_NUM_THREADS=1`).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

https://claude.ai/code/session_016vwxtShKDEGw5Dnb7o3MeB

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b519bd1a157d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.