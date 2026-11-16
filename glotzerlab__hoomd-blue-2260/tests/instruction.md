# Issue

**Title:** Build with rocm in GitHub Actions.

## Description

## Description

<!-- Describe your changes in detail. -->
Build with rocm in GitHub Actions.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
Test that HOOMD-blue can build with rocm.

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
CI checks.

<!--- Please build the sphinx documentation and check that any changes to
      documentation display properly. -->

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b6fe9697a175`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.