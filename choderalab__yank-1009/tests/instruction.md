# Issue

**Title:** Add option to force full trajectory analysis

## Description

Added CLI and API options to force analyzer to use the full trajectory instead of the auto-equilibration and decorelation of the trajectory to compute the free energy.

Decorrelation data can still be computed, but any thing which creates the MBAR object or anything from that automatically will use the full trajectory

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b7518b515789`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.