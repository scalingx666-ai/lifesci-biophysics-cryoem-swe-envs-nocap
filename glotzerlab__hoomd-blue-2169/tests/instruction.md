# Issue

**Title:** Initialize rigid body constituent particle mass

## Description

## Description

The method `create_bodies` allows for the masses of constituent particles to be set when generating them. Setting masses is optional and uses the same format as setting charge.

## Motivation and context



## How has this been tested?

Pytests have been updated to test and use this functionality.

## Checklist:

- [X] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk-minor/CONTRIBUTING.rst).
- [X] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk-minor/ContributorAgreement.md).
- [X] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `94c61b690517`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.