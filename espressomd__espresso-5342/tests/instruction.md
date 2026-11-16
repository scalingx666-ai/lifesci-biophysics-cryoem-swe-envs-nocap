# Issue

**Title:** core/io: fix H5MD time/step datasets off-by-one vs value datasets

## Description

`create_dims()` for rank-1 datasets (step/time) returned an initial shape
of `{data_dim}` (non-zero), while rank-2/3 value datasets started at `{0,...}`.
As a result `id/step` and `id/time` were seeded with one fill element at index 0,
and every subsequent write landed at offset `extents[0]` of the value dataset,
misaligning the time axis by one frame for the entire trajectory.

**Fix:** return `{0ul}` for rank-1 in `create_dims()`, matching the value
datasets (`src/core/io/writer/h5md_core.cpp`).

**Regression test:** `test_time_step_alignment` added to `H5mdTests` in
`testsuite/python/h5md.py`.

Fixes bug #1 of the adversarial core bug sweep.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7752406b29b7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.