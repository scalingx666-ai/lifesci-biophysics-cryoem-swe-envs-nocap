# Issue

**Title:** Handle old constrained to new unconstrained transformation, and vice versa

## Description

## Description

This handles demapping old constrained atoms that become unconstrained in the new ligand, or vice versa.

This does it by directly checking if the atom changes the constrained state during the transformation and deletes de index from the mapping.

This also handles rendering the atom mapping correctly after correction.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Ligands with atoms changing constrained status were not being handled by mapping proposal. Atoms in bonds that change constrained/unconstrained to unconstrained/constrained during the transformation are now _demapped_.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a95bf8d8c0be`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.