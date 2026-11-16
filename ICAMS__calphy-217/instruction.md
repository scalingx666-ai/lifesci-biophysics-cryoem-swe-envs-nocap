# Issue

**Title:** Add conditional import for trapz based on NumPy version

## Description

Import trapz function conditionally based on NumPy version.

It's been finally removed in 2.4, but since numpy version is not restricted in pypi/conda packages, it's currently possible to end up with broken calphy installations.

https://numpy.org/doc/stable/release/2.4.0-notes.html#removal-of-deprecated-functions-and-arguments

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87684b34773b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.