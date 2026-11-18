# Issue

**Title:** rename `_update_nodes_with_xyz`

## Description

renamed `_update_nodes_with_xyz` to `compute_compartment_centers` which also exposes it.
Also changed the toggle `update_nodes` in the movement methods to `False`, to not confuse the user if they move a module and suddenly the comp centers appear in nodes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6a1e17e94838`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.