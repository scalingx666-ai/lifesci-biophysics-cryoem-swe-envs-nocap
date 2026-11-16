# Issue

**Title:** Add convenience methods to extract ligand trajectories by replica

## Description

* Adds a new subclass `HREXSimulationResult(SimulationResult)`, moves HREX-specific fields from generic `SimulationResult`
* Adds convenience methods:
  - `extract_trajectories_by_replica(atom_idxs)`
  - `extract_ligand_trajectories_by_replica()`

  to `HREXSimulationResult`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55c3633e7750`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.