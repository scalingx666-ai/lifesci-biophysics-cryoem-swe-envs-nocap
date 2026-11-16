# Issue

**Title:** OpenMMEngine: Persist `platform`; allow `openmm_properties` override in `initialize`

## Description

This allows OpenMM engines to persist their platform (if given to `__init__`). It also allow override of `openmm_properties` (and `platform`) in the `initialization()` method.

Resolves part of #1177, following the approach proposed at

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `45f4f4086713`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.