# Issue

**Title:** Port PDBx/mmCIF Reader/Writer from OpenMM to MDTraj

## Description

This pull request ports the PDBx/mmCIF file reader and writer from OpenMM into MDTraj, and removes the OpenMM requirement for cif file reading and writing. Most of the dependencies, such as `element` and `Topology`, were already present in MDTraj with slight modifications.

This PR corresponds to issue #1998 

### Changes:  
- Introduced `.pdbx`, `.cif`, `.pdbx.gz`, and `.cif.gz` extensions in `mdtraj/core/trajectory.py`.  
- Ported PDBx container and parsing logic from OpenMM:  
  - `PdbxContainers.py` (mmcif container class)
  - `PdbxReader.py` (mmcif file reader)  
  - `pdbx.py` (loading PDBx/mmCIF files and PDBxTrajectoryFile)  
  - `pdbxfile.py` (Reading and writing interface)  
- Removes OpenMM dependency from `tests/test_pdbx.py`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf11428b49c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.