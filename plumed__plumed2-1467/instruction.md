# Issue

**Title:** Reject invalid OPES deposition strides and kernel widths

## Description

## Summary

Reject non-positive or non-finite kernel widths and minimum widths, non-positive/out-of-range deposition strides, and invalid temperatures during OPES input parsing. These values otherwise reach divisions, modulo operations or invalid kernels later in a run.

This targets v2.9, the oldest maintained affected series, and applies to OPES_METAD and OPES_METAD_EXPLORE. Valid-input behavior is unchanged.

## Tests

An independent native v2.9 C++14 MPI build passed the new input-validation regression (12 rejection cases), together with the existing OPES_METAD and OPES_METAD_EXPLORE regressions. Existing references and tolerances were unchanged. Bundled astyle and whitespace checks passed.

## CI follow-up

The independently reviewable test-initialization fix #1464 is now included in this branch. It initializes the existing timestep regression cell/virial; the deterministic poisoned-stack negative/positive controls show the original test fails and the corrected test matches the original references. No production behavior or tolerance is changed by that companion commit. GNU Autoconf download timeouts in `codecheck` occur before source checks and remain an external CI limitation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d650dbd5a52e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.