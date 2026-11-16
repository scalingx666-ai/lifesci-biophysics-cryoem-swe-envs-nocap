# Issue

**Title:** Preserve `chain_ids` for `to_openmm` and `from_openmm`

## Description

Hi all,

This PR perserves `chain_id`s when going to/from openmm topologies. It is pretty analogous to  

This allows preservation of chain IDs when reading/writing PDBx/mmCIF files, since, for reading, `PDBXTrajectoryFile.__init__` uses `from_openmm`, leveraging openmm's `PDBxFile` object that maintains chain IDs itself,  and, for writing, the `PDBxTrajectoryFile.write` method uses `to_openmm`.

The `topology` property, and the `topology.setter` in `HDF5TrajectoryFile` need extra work to carry through chain IDs in topologies there since they use a `topology_dict` that doesn't carry through chain IDs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bc0d5391361e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.