# Issue

**Title:** Improve performance of Cython script interface for nested array properties

## Description

These changes to the Cython script interface improve performance of the conversion between nested numpy arrays and nested vectors, which are used in every property read and write access of particle slices for vector properties (like `pos`, `v`, ...).

- Added special case in `python_object_to_variant` for efficient conversion of nested numpy arrays to vector of variant vectors.

- Added special case in `variant_to_python_object` for efficient conversion of vector of Vector3d containers to numpy arrays.

- Removed unnecessary copying of value array in `get_attribute` method of `ParticleSlice` python class.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9e7d13d38042`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.