# Issue

**Title:** Remove SIGTERM

## Description

## Description

<!-- Describe your changes in detail. -->
Remove `SIGTERM handler`.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
Compatibility.

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
Existing tests.

<!--- Please build the sphinx documentation and check that any changes to
      documentation display properly. -->

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6269d38a14b7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.