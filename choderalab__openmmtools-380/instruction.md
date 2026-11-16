# Issue

**Title:** Reimplement exact PME to use parameter offset in OpenMM 7.3

## Description

This removes the old implementation of exact PME that used `updateParametersInContext` in favor of using the parameter offset feature offered in OpenMM 7.3.

`AlchemicalState` now is a very short extension of `GlobalParameterState` that provides a general way to implement composable states controlling global parameters. I've also added `GlobalParameterFunction`, which is the general version of `AlchemicalFunction`.

I've also removed support for Python 2 here.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5d2c31d5f540`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.