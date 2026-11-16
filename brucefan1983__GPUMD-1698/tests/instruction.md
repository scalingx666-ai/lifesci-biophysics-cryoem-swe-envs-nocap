# Issue

**Title:** Refactor add spring

## Description

## Summary

Refactor `add_spring` to use the common `Action` framework and simplify its state and restart management.

## Modification

* Refactored `Add_Spring` as an independent `Action` managed by `Measure`.
* Removed the special `add_spring` handling from `Run` and the fixed maximum number of spring commands.
* Introduced persistent spring IDs for restart/continuation.
* Simplified per-spring state management and output/restart file naming.
* Continued runs append to the existing spring output instead of overwriting it.

## Licensing

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

## Validation

Built and tested the refactored implementation in `-DDEBUG` mode.

## Others

None

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2a226a84741`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.