# Issue

**Title:** Read a missing PDB insertion code as an empty string

## Description

PDBFile gave residues an insertion code of ' ' when the file did not specify one, while Topology.addResidue() and PDBxFile use ''.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35ba12a95586`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.