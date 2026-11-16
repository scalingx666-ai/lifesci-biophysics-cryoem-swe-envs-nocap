# Issue

**Title:** Parallelize PIMD bead operations

## Description

### Summary

Parallelize several bead-level operations in the PIMD implementation while preserving the existing numerical behavior.

### Changes

* Parallelize periodic-boundary unwrapping across PIMD beads.
* Parallelize center-of-mass momentum correction across atoms and beads.
* Parallelize bead-data loading for position, velocity, force, potential, and virial averaging while preserving the original bead reduction order.
* Preserve the existing float-based PBC behavior and bead-0 reference convention.

### Validation

* Full regression suite: 189/189 cases passed.
* Regression relations: 23/23 passed.
* Extended PIMD tests with 2, 4, 8, 16, 32, 64, and 128 beads all passed.
* Extended-test outputs were bitwise identical to the current master implementation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c76ae0642920`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.