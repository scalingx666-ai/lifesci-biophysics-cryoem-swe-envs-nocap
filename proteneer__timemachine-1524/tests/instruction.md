# Issue

**Title:** Refine selection of atoms in REST region using simple heuristic

## Description

The initial implementation of REST in single topology transformations in #1424 applies temperature scaling to all ligand-environment nonbonded interactions, and all ligand-ligand proper torsions that are rotatable or in aliphatic rings. I.e., the "REST region" (here, the set of ligand atoms whose interactions may potentially be scaled), is implicitly _all_ of the combined ligand atoms.

This PR refines the REST region using a simple heuristic: a combined ligand atom in included iff it is involved in a bond, angle, or improper torsion interaction whose parameters differ between the end states of the transformation. The logic for scaling the relevant interactions is retained from the previous version, except that we now exclude interactions from scaling that do not involve an atom in the REST region.

This is intentionally a very minimal heuristic. Later on, we might extend this for example to include in the REST region ligand atoms that are not directly mutated in the transformation, but are nearby (e.g. by graph distance) to a mutation.

This PR also adds a utility to visualize the REST region using `py3Dmol` (similar to the existing `view_atom_mapping_3d`). Example from the hif2a public benchmark:
<img src=https://github.com/user-attachments/assets/a56ea806-a5fd-49c1-8fd4-7527435cb5cc width=500/>


TODO:
- [x] add tests exercising heuristic
- [x] rebase once #1523 is merged

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f8756c8fdae9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.