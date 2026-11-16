# Issue

**Title:** Specifying energy components by name, using dictionaries.

## Description

## Description

The function `compute_potential_components` is now using dictionaries to look for components by name, avoiding changes in the order by nightly and upcoming `openmm` versions.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Tested locally.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Energy components are now accessed by name, using dictionaries in `compute_potential_components`.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e42fb89e7b7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.