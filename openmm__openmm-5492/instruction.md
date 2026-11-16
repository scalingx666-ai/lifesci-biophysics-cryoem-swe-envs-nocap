# Issue

**Title:** Keep exact positions when reorderAtoms wraps a molecule

## Description

reorderAtoms() wraps molecules that leave the periodic box back inside it. In mixed precision the wrap subtracted a float offset from posq and left posqCorrection alone, so a wrapped atom lost the extra precision it held, and positions from getState() moved by L - float(L) on each wrap.

This PR shifts each wrapped molecule by whole double precision box vectors, the same ones getPositions() uses to undo the wrap. In mixed precision it subtracts the shift from posq + posqCorrection in double and splits the result back into the two. testReordering now checks positions to 1e-12 outside single precision, and with master's wrap code it fails in mixed precision.

Tested on my M3 Ultra: OpenCL single (NonbondedForce, Settle, VerletIntegrator, CustomIntegrator, LangevinMiddleIntegrator, Checkpoints, PythonForce, Rpmd), Reference and CPU NonbondedForce, and Metal mixed precision with my df64 branch (NonbondedForce, LangevinMiddleIntegrator).

Built and tested with Claude Opus 5.5.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ce8d28f5385`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.