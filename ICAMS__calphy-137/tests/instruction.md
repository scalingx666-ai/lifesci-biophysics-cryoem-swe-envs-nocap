# Issue

**Title:** Fix deprecated scipy.integrate.cumtrapz

## Description

I replaced deprecated scipy.integrate.cumtrapz  in integrators with the  new scipy.integrate.cumulative_trapezoid in a backwards compatible way.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dd581ecf961c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.