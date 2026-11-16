# Issue

**Title:** Amortize PME atom-grid re-sort for large systems

## Description

Systems >15000 atoms re-sort the PME atom grid every step (CommonCalcNonbondedForce), bypassing the step-counter amortization used below that threshold. The sort only reorders atoms for charge-spread atomicAdd locality — it doesn't affect results beyond floating-point summation order — so the every-step cadence is mostly wasted work on current hardware. This re-sorts large systems every 2 steps instead; ≤15000 atoms keep the existing every-4 cadence.
```
  -            if (stepsToSort <= 0 || doLJPME || cc.getNumAtoms() > 15000) {
  +            if (stepsToSort <= 0 || doLJPME) {
                   ...
  -                stepsToSort = 3;
  +                stepsToSort = (cc.getNumAtoms() > 15000) ? 1 : 3;

```
The every-2 staleness bound is strictly tighter than the every-4 already shipping for ≤15000-atom systems.

  Measured (paired, 6 reps/cell, TIP3P/PME, >15000 atoms):

  - CUDA (T1000/Turing, H100/Hopper, RTX 5090 + PRO 6000/Blackwell): +1–4%, increasing with system size, no regression.
  - OpenCL: ~+3%.
  - HIP (W7900/RDNA3, RX 9070 XT/RDNA4): positive to ~10⁵ atoms, ~−0.7% at the largest systems (within run-to-run noise).
  - Double precision ~flat (atomicAdd-throughput bound). <15000 atoms unchanged (identical code path).

Thanks

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35d4ce904f2c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.