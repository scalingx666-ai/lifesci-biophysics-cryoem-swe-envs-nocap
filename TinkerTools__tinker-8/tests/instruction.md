# Issue

**Title:** Fix omm

## Description

This fixes a number of issues with the OpenMM integration:
- Makes sure unit cell vectors are set correctly for non-orthogonal cells
- Resolves a bunch of compiler warnings associated with structs that had no external linkage (fixed by making them `static`)
- Resolves more compiler warnings about assigning to `char*` via `=` (you can do this with `std::string`, but not `char*`)
- Adds the functionality to write out a `system.xml` file that can be used with OpenMM in standalone mode if you request 0 steps with `dynamic_omm`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `de43b62aeb66`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.