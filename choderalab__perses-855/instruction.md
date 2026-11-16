# Issue

**Title:** Fix geometry engine bug

## Description

## Description

I found a bug when playing around with the geometry engine. I think the changes in this PR are necessary because if `topology_index_map` is `None`, then line 2232 will throw an error because you cannot retrieve four indices from an object that is `None`. Therefore, line 2232 and subsequent lines must be indented to be within the `if topology_index_map is not None:` statement.

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dfc970f62319`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.