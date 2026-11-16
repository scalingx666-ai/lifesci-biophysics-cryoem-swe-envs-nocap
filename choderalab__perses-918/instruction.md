# Issue

**Title:** Fix atom masses in vanilla HybridTopologyFactory

## Description

## Description
Takes the average of old and new system masses for mapped (environment/core) atoms.

<!-- Describe your changes in detail. -->

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
Currently, the `HybridTopologyFactory` adds all environment/core/unique old atoms to the hybrid system using the mass of the particle in the old system. It then adds unique new atoms using the mass of the particle in the new system.

To improve stability, we want to average the old and new system masses for any mapped atom (environment/core atoms). 

<!-- Replace ??? with the issue number that this pull request resolves. -->


## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b67a7d5f762e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.