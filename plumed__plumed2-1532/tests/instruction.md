# Issue

**Title:** Fit to template performance

## Description

# Branch `fit-to-template-performance` (2026-10-29)

Two commits on top of `datasharing-revision` (c4037ce), see datasharing-revision-plan.md. In /Users/carlo/Codes/plumed2 as local branch `fit-to-template-performance`.

1. 2e3fd9d Value: make addForce inlinable. Value::addForce(index,force) was out of line in Value.cpp and its assertion built an error string, so it was never inlined (~37 instructions per call in callgrind). Common case now inline in Value.h; sparse-matrix case (addForceToSparseMatrix) and the error (addForceOutOfRange, [[noreturn]]) out of line. Helps every action adding forces element by element (ActionAtomistic::addForce, SUM, FLATTEN, CONCATENATE, VSTACK, SELECT_WITH_MASK, TRANSPOSE, ...).
2. 04d8e71 FIT_TO_TEMPLATE and RESET_CELL: loop directly on all positions and forces. New ActionAtomistic methods translateAllGlobalPositions, rotateAllGlobalPositions (with and without shifts), getTotalForceOnAllAtoms, rotateForcesOnAllAtoms, looping on the arrays of the position/force values. Rotation written out with sums starting from 0.0 and in matmul order, forces updated as f+(nf-f): results bitwise identical (forces + full virial of rt63, rt63b, rt63c, rt63d, rt63e at 17 digits identical to master). FIT_TO_TEMPLATE OPTIMAL computes only the diagonal of matmul(ww, dRot/dpos), same summation order. Changelog line added.

@GiovanniBussi @gtribello this PR is optimising FIT_TO_TEMPLATE because in Francesco example this is now accounting for 50% of plumed slowdown. The new performances are as follows:

**Cudacoord: 290 ns/day** (from the original 34 ns/day of master, 200 ns/day from cuda optimisation, 240 ns/day from data-sharing optimisations)
**coordination: 520 ns/day** (from the original 340 ns/day of master, 353 ns/day from pure data sharing, 392 ns/day from NL optimisations related to data-sharing).

now this shows that CUDACOORDINATIONFLOAT is a factor of 1.8 slower than COORDINATION on this specific setup. The changes in this branch will also benefit vectorial CVs (and for example the openACC examples) because of the inlining of the addForce stuff.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b5b0b52c264d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.