# Issue

**Title:** Fix CustomNonbondedForce computed values without global parameters

## Description

A CustomNonbondedForce with computed values but no global parameters gives wrong energies and forces on OpenCL, CUDA and HIP. `execute()` always sets the computed values kernel's globals argument, but the kernel only has that argument when the force has global parameters, so the call replaces the first per-particle parameter. It came in with #5002, so 8.4.0 onward has it. `testComputedValues` uses a global parameter, which hides it.

The fix sets the argument only when `needGlobalParams` is true. The new test checks two particles with a computed value `s = 2*q` against the exact energy (16 kJ/mol). On my M3 Ultra it fails on master on OpenCL and passes with the fix. I couldn't run CUDA or HIP, but they use the same code.

Built and tested with Claude Opus 5.5.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `30dfdc25e576`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.