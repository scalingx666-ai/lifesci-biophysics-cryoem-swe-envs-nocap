# Issue

**Title:** Chain ID is now a string as of cpptraj version 6.27.0

## Description

Chain ID is now a string in the Residue class to accommodate multi-character residue IDs, as well as better support segment IDs in CHARMM PSF files. See

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a318898a88d3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.