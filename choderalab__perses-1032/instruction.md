# Issue

**Title:** Recovering logging capabilities

## Description

## Description

Adding the import line from `perses.multisate` module recovers the logging. I don't really understand why is this the case, but I could trace it back to this change, even if the imported classes are not directly used in the code.

This isn't really a fix, just a quick solution/workaround I could come up with, to the annoying inconvenience of not having logs, especially annoying for debugging purposes.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Tested locally with both `perses-relative` and `perses-cli` entry points.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Recovering logging capabilities by adding `perses.multistate` import to the root entry point.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d0bec4d82259`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.