# Issue

**Title:** Accept multiple ligand input files for `RelativeFEPSetup`

## Description

## Description

`RelativeFEPSetup` now checks wether the `ligand_input` parameters is a list or a string and handles both cases accordingly. This bug was introduced when we changed the behavior of reading files in #1073 .

Added test to check for this behavior.

## Motivation and context

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

Wrote test that checks the length of the `._ligand_input` private attribute for the `RelativeFEPSetup` object.

## Change log

There is no need for a changelog entry here, since the conflicting changes never made it to a release. That is, we are fixing the conflicting changes in the same release.

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aa929f1f7f2f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.