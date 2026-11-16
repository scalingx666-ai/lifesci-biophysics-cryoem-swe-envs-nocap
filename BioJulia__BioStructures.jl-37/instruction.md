# Issue

**Title:** Fix Kabsch Algorithm to Prevent Reflections

## Description

# Fix Kabsch Algorithm to Prevent Reflections

## Types of changes

This PR implements the following changes:
_(Please tick any or all of the following that are applicable)_

* [ ] :sparkles: New feature (A non-breaking change which adds functionality).
* [x] :bug: Bug fix (A non-breaking change, which fixes an issue).
* [ ] :boom: Breaking change (fix or feature that would cause existing functionality to change).

## :clipboard: Additional detail

Before this change, the `rot` field of `Transformation` could be a reflection instead of a pure rotation.

This change ensures that `rot` will be a pure rotation, with determinant 1.

See https://en.wikipedia.org/wiki/Kabsch_algorithm#Computation_of_the_optimal_rotation_matrix

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `876de6bf9285`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.