# Issue

**Title:** Register Boost.Python converters for NumPy 2.0 scalar types

## Description

Workaround for 

NumPy 2.0 scalar types (np.float32, np.int32, np.int64, etc.) no longer subclass Python's built-in float/int, so Boost.Python's rvalue converters fail to convert them. Register custom from-Python converters that use the PyNumber_Float/PyNumber_Long protocols instead.

Fixes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c657b13e93b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.