# Issue

**Title:** Minor improvements to PolymerProposalEngine

## Description

## Description


<!-- Describe your changes in detail. -->
Here, I am adding the following improvements to `PolymerProposalEngine`:
1. For protein mutations, do not map sidechain atoms (CB atoms and beyond)
2. Speed up the execution of `construct_atom_map`
3. Handle ring amino acids

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
Changes:
1. We don't want to map sidechain atoms for protein mutations because of the sidechain rotamer sampling problem. We can use REST to enhance sampling if we do not map sidechain atoms.
2. We can speed up the execution of `construct_atom_map` by removing one of the nested for loops.
3. We need to set `allow_ring_breaking=False` for ring amino acids.


## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
As long as the tests in `test_topology_proposal.py` run ok, we should be fine.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `45e109255974`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.