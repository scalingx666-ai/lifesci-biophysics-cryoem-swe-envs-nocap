# Issue

**Title:** fix numpy 1.19 DeprecationWarning: tostring()

## Description

Since `numpy=1.19` `array.tostring()` is deprecated, and should be replaced with `.tobytes()` (present in `numpy >=1.9`), this PR does that for OPS.

The leftover `>40000` warnings in the test suite come from `NetCDF4` which has been fixed in  (I tested this locally and it passes without these DerpecationWarnings).

Also cleaned some `pep8` complaints on the files I touched.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `12bc6054e360`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.