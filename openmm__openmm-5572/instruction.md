# Issue

**Title:** Optimizations to updateParametersInContext()

## Description

This is a replacement for #5430.  See there for further discussion.

This implements a set of optimizations to `updateParametersInContext()`.

1. When ComputeContext identifies sets of identical molecules, it records the ranges of particles and particle groups for each set.  It also adds another version of `invalidateMolecules()` that lets you pass in the ranges of particles and particle groups that may have changed.  This allows it to skip rechecking groups of molecules that are guaranteed not to have changed.
2. A common case is that only unique molecules (which are unaffected by reordering) such as ligands have been modified.  I added an initial check for that case.  It can then return immediately without waking the worker threads, which avoids overhead.
3. NonbondedForce no longer recomputes the Ewald self energy from scratch.  Instead it subtracts off the old contribution from the old parameters, then adds in the new contribution from the new parameters.  When changing only a small part of the system, this is much faster.  The energy is now accumulated in fixed point so the old contribution can be exactly removed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ef4294d0545e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.