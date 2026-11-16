# Issue

**Title:** Parallelize PIMD centroid virial calculation

## Description

### Summary

Parallelize the centroid-virial calculation in the PIMD implementation across atoms and beads.

### Changes

* Parallelize the per-bead centroid-virial contributions.
* Use shared memory to collect bead contributions for each atom.
* Preserve the original bead reduction order for the final virial and kinetic-energy estimator.
* Remove the unused box argument from the kernel.

### Validation

* Full regression suite: 189/189 cases passed.
* Regression relations: 23/23 passed.
* Extended PIMD tests with 2, 4, 8, 16, 32, 64, and 128 beads all passed.
* Most extended cases were bitwise identical to master. The largest observed difference in the remaining cases was `8.67e-19` absolute (`2.82e-16` relative).
* Paired/interleaved EAM benchmarks with five repeats showed geometric-mean speedups of 1.022× for PIMD and 1.013× overall.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5d3724efd9a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.