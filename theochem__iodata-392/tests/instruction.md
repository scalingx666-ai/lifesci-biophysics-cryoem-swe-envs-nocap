# Issue

**Title:** Update for ORCA log outputs

## Description

Hi! This PR integrate the atomics gradients in ORCA output files as an optional attribute in IOData. Data is stored in Hartree/Bohr units.

## Summary by Sourcery

Add support for reading atomic gradients from ORCA output logs into IOData objects.

New Features:
- Expose atomic gradients from ORCA output files as the optional atgradient attribute on loaded IOData objects.

Tests:
- Add regression test and sample ORCA output file to verify loading of atomic gradients alongside existing properties.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa4d09dfe62a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.