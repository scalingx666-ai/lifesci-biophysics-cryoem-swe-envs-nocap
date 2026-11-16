# Issue

**Title:** optimization of ATMForce kernels

## Description

This PR reduces the overhead of calling the ATMForce GPU kernels raised in PR #4776 and discussion #5051.

The variable displacement enhancement added three kernels to the two (`hybridForce` and `copyState`) that were already there:
- `setDisplacements`: assign displacement vectors to particles (fixed or variable)
- `resetDisplForce`: set to zero to buffer that holds the component of the gradients due to variable displacements
- `displForce`: calculates the component of the gradients due to variable displacements

I was able to get rid of `setDisplacements` by merging it with the existing `copyState` kernel. The particle displacement is set on the fly in local registers when the particle coordinates of the inner contexts are set. This has the added benefit of not storing displacements in the temporary buffers `displ0` and `displ1`, which have been removed.

I was not able to identify further obvious optimizations. `resetDisplForce` must necessarily complete before `displForce` because `displForce` fills the corresponding buffer in random order due to atom reordering, and because it atomically updates the gradients of the particles that determine the displacement of the current particle. Similarly, `displForce` must complete before `hybridForce` because `hybridForce` accesses the buffers in sequence rather than randomly. Optimizations aimed at skipping kernel calls that upset the efficient sequential memory access of `resetDisplForce` and `hybridForce` might not lead to significant benefits. I'm open to suggestions.

Anyway, this reduces the additional kernel calls from 3 to 2, replaces global memory access with registers, and removes two unnecessary buffers.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c91894e8d477`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.