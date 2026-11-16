# Issue

**Title:** Geometry engine platform usage cleanup

## Description

## Description

These changes make the `CPU` platform to be the default platform for the geometry engine.

While working on these changes, it was noted that some of the utility functions lived in the wrong module under `perses.tests.utils`, they were moved to a "better" module in `perses.dispersed.utils`, since we should reserve the `perses.tests.utils` module for things that are only required in our tests.

Backward compatibility is achieved by having variables in `perses.tests.utils` to point to the new `perses.dispersed.utils` functions as needed. This is to avoid breaking workflows of users calling these functions directly from `perses.tests.utils`,

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
It was noted in  that we didn't want to change the default platform in general for that module, but just to change the platform used in the geometry engine.

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
Tested locally, verifying that the `CPU` platform is exclusively used when generating HTFs. This also gives better performance than using `CUDA` platform for "big" and "small" systems.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
Should be the same as in  adding the following
```
Importing utility functions from ``perses.tests.utils`` for production code is now deprecated and will not be a possibility in the upcoming 0.11 release.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c96e65572b26`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.