# Issue

**Title:** Use SYNC_WARPS in the non-PME multipole electrostatics tile loop

## Description

barrier in divergent control flow is UB. Causes deadlocks on Intel GPUs running CUDA/HIP

The following is AI-generated (verified against the code and by running the tests and benchmarks):

. Replaces the two SYNC_THREADS in the per-warp tile loop of computeElectrostatics (non-PME multipole) with SYNC_WARPS, matching the tiles-with-exclusions loop above it and the same loop in pmeMultipoleElectrostatics.cc. The barrier only protects the current warp's own localData entries, so a warp-level sync is enough.

TestCudaAmoebaGeneralizedKirkwoodForce, TestCudaAmoebaMultipoleForce and the HIP equivalents pass in single and mixed precision with this change. examples/benchmarks/benchmark.py, 6 interleaved runs of 30 s each, median ns/day before and after:

```
CUDA GTX 1650   amoebagk   mixed   6.021 -> 6.051
CUDA GTX 1650   amoebagk   single  6.031 -> 6.069
CUDA GTX 1650   amoebapme  mixed   4.690 -> 4.689  (control)
HIP  W6400      amoebagk   mixed   9.122 -> 9.150
HIP  W6400      amoebagk   single  9.280 -> 9.311
HIP  W6400      amoebapme  mixed   2.718 -> 2.711  (control)
```

This PR was generated using AI

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e1c39e7844b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.