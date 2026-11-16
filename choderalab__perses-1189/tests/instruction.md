# Issue

**Title:** Update comments in `RESTCapableHybridTopologyFactory`

## Description

## Description

<!-- Describe your changes in detail. -->
I caught some issues with the comments in the RESTCapableHybridTopologyFactory. I cleaned up the comments in this PR. I also removed a redundant type check in `get_rest_identifer` -- the type is being checked twice, so I removed one of them.

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `186375de6064`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.