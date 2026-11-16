# Issue

**Title:** Fix PairwiseDistances MPI implementation

## Description

`evaluate()` ran only on rank 0 and called `get_local_particle(pid)->pos()`
for every pair.  `get_local_particle()` returns `nullptr` for particles
resident on another MPI rank, causing SIGSEGV on any multi-rank run where
observable particles are not all ghosted onto rank 0.

**Fix:** on all ranks collect `(pid, position)` pairs for locally-owned
particles, gather to rank 0 via `boost::mpi::gather`, build a
`pid→position` map, and compute distances there
(`src/core/observables/PairwiseDistances.hpp`).

**Regression test:** `PairwiseDistancesMultiRankTest` added to
`testsuite/python/observables.py` — subprocess under `mpiexec -n 2` asserts
`calculate()` completes without SIGSEGV.

Fixes bug #21 of the adversarial bug sweep.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6da94c432a43`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.