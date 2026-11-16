# Issue

**Title:** Make add_random_force as a property

## Description

## Summary

Refactor `add_random_force` as a `Property`.

## Modification

- Make `Add_Random_Force` inherit from `Property`.
- Execute it through the `post_force()` callback.
- Remove the redundant `num_calls_` state and the special handling in `Run`.
- Move the source files to `src/measure/`.

The input syntax and normal runtime behavior are unchanged.

For duplicated `add_random_force` commands, the error is now handled by the common `Measure` duplicate-property check.

## Validation

Regression tests cover:
- NVE without `add_random_force`
- single `add_random_force`
- consecutive runs
- interaction with `add_force`
- duplicate-command rejection
- deform callback regression

All normal-output cases are bytewise identical to the previous implementation.

## **Licensing** (Please read and confirm before submitting this pull request)

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8de022e99b00`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.