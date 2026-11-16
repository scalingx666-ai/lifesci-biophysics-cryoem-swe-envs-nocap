# Issue

**Title:** Rename second CustomBondForce in HybridTopologyFactory system and update docstrings

## Description

## Description

- Renames second `CustomBondForce` in `HybridTopologyFactory` system to `CustomBondForce_exceptions`
- Updates docstring in `HybridTopologyFactory` to contain the list of forces present in the hybrid system
- Fixes docstring for RESTTopologyFactory to accurately reflect the forces in the system 

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
This change is necessary because  changes `compute_potential_components()` to use a dictionary where the keys are force names and values are energies.

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `66ab55888731`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.