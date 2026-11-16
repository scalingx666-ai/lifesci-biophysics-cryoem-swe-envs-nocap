# Issue

**Title:** Simplify `_construct_atom_map` for protein mutations

## Description

## Description
I've simplified the code for generating protein mutation atom mappings by generating the mapping directly from the old/new residue topologies. Generating the atom mapping no longer involves using `AtomMapper` or oemols, but oemols are still generated and attached to the output `TopologyProposal` because they are needed as input to the geometry engine.

By default, we map backbone atoms and CB atoms (except for GLY, where we map CA but not HA2 or HA3).

<!-- Describe your changes in detail. -->

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
I added `test_protein_atom_maps()` to `test_topology_proposal.py`, which tests the generation of atom maps for some representative transformations.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e165e760eeb4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.