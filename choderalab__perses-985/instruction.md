# Issue

**Title:** remove dead lines in test + keep topology in openmm if we can

## Description

## Description

Fix issue with GPU tests where we didn't handle solvated topologies correctly.

## Motivation and context

See 

<!-- Replace ??? with the issue number that this pull request resolves. -->
Resolves 975

## How has this been tested?

@ijpulidos can you test this locally on a GPU? Right now my laptop graphics drivers are borked 

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `285205de4c15`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.