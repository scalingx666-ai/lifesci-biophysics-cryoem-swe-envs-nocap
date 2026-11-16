# Issue

**Title:** Speed up probe2: direct arithmetic in hot loops, cache dot sort keys

## Description

## Summary
Speed up `probe2` by removing per-operation `scitbx.matrix.rvec3()` allocations from the hot loops and caching dot sort keys.

- Replace `rvec3()` vector operations with direct tuple arithmetic in the distance checks and dot/spike location calculations, avoiding millions of Python matrix-object allocations.
- Cache each `DotInfo` sort key at construction time to eliminate repeated `_makeName` calls during sorting.

~23% faster on 6m4j (30s → 23s).

## Correctness
Pure performance refactor — output is unchanged. Verified by running `mmtbx.probe2 approach=self` on a hydrogenated model (1nxb) before and after the change: the two output files are **byte-identical** across all 34,744 dot lines (only the caption timestamp/filename differ).

Regression tests pass locally: `mmtbx/validation/regression/tst_probe2_asu_bond_proxies.py` and `mmtbx/probe/tst_mmtbx_probe_ext.py`.

## Notes
Single commit, touches only `mmtbx/programs/probe2.py`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd1477cd9ba1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.