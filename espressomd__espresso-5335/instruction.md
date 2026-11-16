# Issue

**Title:** core/accumulators: fix Correlator::finalize() discarding compression results

## Description

In `finalize()` the compress calls did not assign their return values, so
the higher-hierarchy buffer slots (`A[i+1][newest[i+1]]`,
`B[i+1][newest[i+1]]`) were never updated.  The subsequent correlation loop
read stale ring-buffer data into the long-lag-time tail, silently corrupting
results for any multi-block correlator (`hierarchy_depth >= 2`, the standard
`tau_max/dt >= tau_lin` case).

**Fix:** assign the compress results in `finalize()` (lines 477-480 of
`src/core/accumulators/Correlator.cpp`), mirroring the identical assignment
already present in `update()`.

**Regression test:** `test_finalize_preserves_long_lag_tail` added to the
`CorrelatorTest` class in `testsuite/python/accumulator_correlator.py`.

Fixes bug #2 of the adversarial bug sweep.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6838ba6148e0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.