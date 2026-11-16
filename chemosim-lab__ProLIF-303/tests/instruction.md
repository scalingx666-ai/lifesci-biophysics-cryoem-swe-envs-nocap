# Issue

**Title:** GSoC visualisation improvements

## Description

Updates:
1. Modularise the `lignetwork.py` file to a network package that contains `lignetwork.py`, `network,js`, `network.css`, `network.html`
2. Convert Fingerprint data to NetworkX graph object.
3. Coordinate Calculation to organize residues (protein and water) and ligand placements.
4. Tests for calculated coordinates.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cdc7a58ff7ce`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.