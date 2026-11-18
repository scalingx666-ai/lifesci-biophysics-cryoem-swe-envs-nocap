# Issue

**Title:** proposed new api; not tested

## Description

This PR proposes to use as much as possible the existing `init_fn` and `step_fn`.

The only additional thing introduced by Matthijs's PR is now the utility to ravel or unravel the states. For all else, we rely on `init_fn` and `step_fn` from `jx.integrate`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `45aee89d6b3d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.