# Issue

**Title:** Change make_bonds processor to use atom names and keep residues intact

## Description

Make Bonds will now first use atom names to guess bonds within residues. Secondly, distances will be used to add the edges between residues, and involving atoms that are not known to the reference Blocks. 
Make bonds now splits the system in connected components less aggressively: residues are kept together in the same molecule. Tests adapted to reflect this change.

The underlying idea is that the priority of information is CONECT > atom names > distance; and the user should be able to toggle the use of atom names and distance using the CLI (implementation pending). The fallback/nuclear option for the user is then to create a PDB with all CONECT records (make by e.g. -write-graph, and then curated).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fef5392671e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.