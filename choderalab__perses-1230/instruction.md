# Issue

**Title:** Fix/issue 1194

## Description

## Description

<!-- Describe your changes in detail. -->

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Correctly set cutoff for `CustomNonbondedForce` in `HybridTopologyFactory` when using a non-default cutoff distance.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aca858f917f5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.