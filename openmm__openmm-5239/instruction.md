# Issue

**Title:** Cache coefficients for long range correction

## Description

For CustomNonbondedForce to calculate the coefficient for the long range correction, we need to compute a bunch of integrals numerically.  This makes it slow every time you change the global parameters.

This PR makes it cache the coefficients after it computes them.  When you set the global parameters back to values it has seen before, it loads the coefficients from the cache instead of recomputing them.  When you're switching back and forth between a set of discrete states, this can make it much faster.

I limited the size of the cache to 1000 entries so it won't grow without limit, for example if you're doing lambda dynamics so the parameters can take on arbitrary values.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b55e60882dff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.