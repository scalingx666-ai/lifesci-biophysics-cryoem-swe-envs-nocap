# Issue

**Title:** Cleaning up platform and context usage in geometry engine

## Description

## Description
By default the `CUDA` platform was hardcoded in the geometry engine. Generating contexts that were not handled correctly. This PR is aimed towards making `CPU` to be the default platform for the geometry engine and cleans explicitly created contexts for a better memory management.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
``CUDA`` platform was hardcoded in geometry engine, generating performance issues by not clearing openmm contexts correctly. Fixed by defaulting to using the faster ``CPU`` platform (for the geometry engine) and explicitly deleting context variables after they are used.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37da4fb9ad88`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.