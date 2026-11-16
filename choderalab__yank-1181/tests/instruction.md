# Issue

**Title:** Fix reversed direction trailblaze

## Description

This fixes a bug in the `run_thermodynamic_trailblazing` function when the algorithm was run in the reversed direction without alchemical functions.

This also  so that the correct error message is displayed when the anisotropic dispersion correction cutoff is set too high.

~I've also changed the default 'auto' alchemical path to use alchemical functions instead of a sequence of state parameters. This allows us to "cut corners" and avoid forcing states when variables are completely turned off/on if not necessary to maintain a constant thermodynamic length between states. For example, now the state (lambda_electrostatics=0, lambda_sterics=1) is not necessarily generated, but the algorithm can skip to (lambda_electrostatics=0, lambda_sterics < 1) until the standard deviation threshold is hit.~ EDIT: See message below.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2c87ad870604`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.