# Issue

**Title:** Set chunksize for iteration dimension to 1 and doc fixes

## Description

## Description
- Fix #409: Rename `restart_attempts` to `n_restart_attempts` in `BaseIntegratorMove` docstring.
- Fix #418: Rename references to T4Lysozyme+p-xylene to host-guest in the tutorial.
- Fix #408: Add `create_thermodynamic_state_protocol()` to RTD docs.
- Fix #422: Set chunksize for iteration dimension to 1.

## Status
- [X] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2ea991c94a3a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.