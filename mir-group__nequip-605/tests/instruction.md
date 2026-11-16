# Issue

**Title:** Fix torch<->Kokkos GPU stream race in LAMMPS ML-IAP compute_forces

## Description

**Problem.** With `pair_style mliap unified <model> 0` on the Kokkos backend (`mliap/kk`), MD intermittently crashes with a GPU memory access fault (out-of-bounds write). Static `run 0` is always fine — only dynamics, non-deterministically, on non-degenerate cells with multi-layer models. `HIP_LAUNCH_BLOCKING=1` makes it vanish. (Same class as ACEsuit/mace#1294.)

**Cause.** A race between PyTorch's stream and the LAMMPS Kokkos kernels. `compute_forces` reads LAMMPS GPU arrays via `torch.as_tensor(lmp_data.rij / pair_i / pair_j)` on torch's stream, but those arrays are produced by Kokkos kernels on a *different* stream with no synchronization — so torch can read them before Kokkos finishes writing → stale indices → out-of-bounds write. (Confirmed: indices are in bounds, `max(pair_j) < ntotal`, so it's not an index-size bug; and adding a sync removes the fault.)

**Fix.** One input-side sync at the start of `compute_forces`, before torch reads the arrays. 

Overhead was measured to  ~0.8% on an OAM-S model that did not crash on this stream race. 

Verified on MI250X (ROCm 6.3.4, LAMMPS patch_30Mar2026, a 4-layer model): without fix, GPU fault at ~step 28 on a ≥512-atom non-degenerate cell. With fix, stable for 1000+ steps, energy conserved, PE ≈ `pair_style nequip` to ~1e-6, single- and multi-GPU (strong/weak scaling to 8 GPUs).

Could perhaps also be solved by running the Kokkos ML-IAP kernels on torch's stream. Given the modest ~0.8% cost, this one-liner should do a good job in the meantime.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9995ed81413e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.