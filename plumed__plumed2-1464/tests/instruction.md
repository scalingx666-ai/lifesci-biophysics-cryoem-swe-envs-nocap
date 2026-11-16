# Issue

**Title:** Initialize cell and virial in timestep rounding regression

## Description

## Summary

Initialize the cell and virial buffers in the `setTimestep` rounding regression. The test passes both arrays to PLUMED even though it only exercises a constant CV. Uninitialized cell entries can trigger the NaN assertion in `LatticeReduction`, depending on the stack contents.

This was exposed by the Fedora39 CI job for #1462: 627 tests were run and `basic/rt-make-settimestep-rounding` was the sole reported error. The ENSEMBLE change is not used by this test.

## Validation

Using the native regression runner with GCC 12 and `-ftrivial-auto-var-init=pattern`:

- The unchanged test deterministically failed, reproducing the invalid cell.
- The initialized test passed both double and float paths with the existing COLVAR reference.

No expected result or tolerance was changed. The patch targets v2.9, the oldest maintained affected branch, and changes only the test setup.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d650dbd5a52e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.