# Issue

**Title:** Saturate float to fixed point conversions in OpenCL

## Description

.

On Apple M3 GPUs, converting an out-of-range float to `long` wraps modulo 2^64 instead of saturating. Huge forces became 0, so the minimizer's overflow check never fired and `testLargeForces` stopped at its starting point. `realToFixedPoint` now uses `convert_long_sat`.

Tested on an M3 Ultra: all 66 OpenCL tests pass, including `TestOpenCLLocalEnergyMinimizerSingle`, which fails on master.

Using Claude Fable 5.1, I traced the cause and wrote the fix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3c9effc96d0c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.