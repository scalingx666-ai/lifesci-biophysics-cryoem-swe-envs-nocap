# Issue

**Title:** Avoids using Dispersion Correction when Custom Nonbonded forces are used

## Description

Fixes a bug where dispersion correction was still being loaded despite not being defined in XML files using `CustomNonbondedForce`. The bug happened because a mistaken indexing in the `ff_param_array` when the XML was being read and loaded.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0f0dde07d55c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.