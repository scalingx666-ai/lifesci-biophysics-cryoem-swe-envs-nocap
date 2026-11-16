# Issue

**Title:** Add utility method to emit RDKit mol from SingleTopology classes

## Description

Currently, rendering the ligand coordinates in the intermediate windows tends to result in a lot of mangled bond orders and difficult to visualize trajectories. This PR adds a utility function `SingleTopology::mol()` to emit an RDKit mol with a basic attempt at guessing whether or not a bond is present. For tracing replica trajectories in HREX, the SDF format is recommended (over PDB/mmCIF) since the bond orders are read explicitly by common viewers.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7710ecb24b5e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.