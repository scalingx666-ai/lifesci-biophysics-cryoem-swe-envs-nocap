# Issue

**Title:** fix: detach tensors before passing to LAMMPS in ML-IAP wrapper

## Description

(maybe) fixes mir-group/allegro#133

All three tensors passed to LAMMPS at the end of `compute_forces` still carry `requires_grad=True` -`nequip_atomic_energies` and `nequip_total_energy` because they're downstream of `edge_vectors` which has `requires_grad_(True)` set on line 221, and `edge_forces` because it comes from `torch.autograd.grad`.

When thermostats like `fix langevin` add random and drag forces directly to the per-atom force array in-place after each force evaluation, the in-place modification can corrupt PyTorch's computational graph if those arrays are still attached to it. This explains the temperature explosion to 100K+ observed in the linked issue with `fix langevin` but not with `fix nvt`, since Nosé-Hoover modifies velocities rather than the force array directly.

The `UserWarning: Converting a tensor with requires_grad=True to a scalar` on line 178 is a symptom of the same issue.

This fix adds `.detach()` before handing tensors to LAMMPS. It has no effect on correctness since gradients are never needed past this point, and no effect on performance.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f91aa4fcda62`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.