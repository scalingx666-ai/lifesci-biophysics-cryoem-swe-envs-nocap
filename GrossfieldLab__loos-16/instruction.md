# Issue

**Title:** Handling internal waters in omg.py

## Description

Towards the end of system building, the coordinates of the internal/crystallographic waters are copied to some of the bulk waters, so that there is a single water segment. There seems to be a bug where the internal/crystallographic water segment is remove from the PSF, but from the PDB.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa24191ea717`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.