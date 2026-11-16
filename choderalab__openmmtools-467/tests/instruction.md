# Issue

**Title:** Fix an issue with accidentally resetting PeriodicNonequilibriumIntegrator

## Description

We should be accumulating work as we go around the cycle, not resetting the integrator each cycle!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9fa56df507b0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.