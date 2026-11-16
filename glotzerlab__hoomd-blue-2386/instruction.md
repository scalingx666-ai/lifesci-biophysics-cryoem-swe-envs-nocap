# Issue

**Title:** Fix bug preventing typeparamdict defaults being set by OnlyIf

## Description

## Description

Currently, attempting to set default typeparam dicts fails if they are wrapped around OnlyIf objects. This PR fixes that error and adds a unit test to ensure the failure does not occur.

## Motivation and context
N/A

## How has this been tested?

New unit test.

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `93a6699eb6eb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.