# Issue

**Title:** Fix scale tuner

## Description

<!-- Please confirm that your work is based on the correct branch. -->
<!-- Bug fixes should be based on *trunk-patch*. -->
<!-- Backwards compatible new features should be based on *trunk-minor*. -->
<!-- Incompatible API changes should be based on *trunk-major*. -->

## Description

<!-- Describe your changes in detail. -->
Scale move sizes the correct direction when the acceptance rate is 0.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
The tuner currently makes moves *larger* when the acceptance is 0. Thus, the tuner will never converge.

## How has this been tested?

A new unit test checks the behavior of the tuner with 0 acceptance.
<!--- Please describe in detail how you tested your changes. -->

<!--- Please build the sphinx documentation and check that any changes to
      documentation display properly. -->

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk-minor/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk-minor/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc2b62a1fa04`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.