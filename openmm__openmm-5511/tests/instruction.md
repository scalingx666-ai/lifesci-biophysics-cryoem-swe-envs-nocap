# Issue

**Title:** Reference neighbor list: keep a coordinate just below zero in range

## Description

For PME, OpenMM’s Reference neighbour list tries to wrap tiny negative coords into the box, they end up one cell past the grid. Then those coords' pairs are all skipped.

---

*The rest of this description, the fix and the test were written with Claude Opus 5.5 (Anthropic); I have reviewed them and run the test.*

.

`getVoxelIndex` wraps a periodic coordinate with `r -= L*floor(r/L)`, and for a coordinate a hair below zero the result rounds to exactly `L`, giving voxel index `n`. `getNeighbors` only visits `0..n-1`, so the particle's pairs were dropped. The fix clamps the periodic voxel index to `n-1`, the voxel whose upper edge the wrapped coordinate sits on. That keeps the voxel consistent with the coordinate, which matters for triclinic boxes, where wrapping the index to 0 instead would move the particle by a box vector. Distances are still computed with the minimum image, so the voxel only chooses candidates.

`testCoordinateJustBelowZero` puts one of two particles at −1e-17 along each axis in turn, in an orthorhombic and a triclinic box. Before the fix it fails with `Expected 1, found 0`; after it, the whole `TestReferenceNeighborList` passes. I built the test directly against the changed sources rather than the full tree.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d3e32b4daea1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.