# Issue

**Title:** Fix water selection for counterion

## Description

## Description

<!-- Describe your changes in detail. -->

Currently, we choose a water at least X nm from protein to change into an ion. If there are glycan residues, the current implementation is incorrect, as a water may be chosen near a glycan, which we don't want. 

This PR changes the selection string from `protein` to `not water and not resn NA and not resn CL and not resn ZN`. The ions in the selection string may be supplied to the method as a list of strings.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
Resolves #???

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1cf25b0f2928`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.