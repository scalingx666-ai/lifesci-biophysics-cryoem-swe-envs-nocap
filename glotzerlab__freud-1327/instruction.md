# Issue

**Title:** Test on ARM Linux.

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
<!-- Describe your changes in detail. -->
Execute unit tests on ARM Linux. Also bump the test dependencies to the latest.

## Motivation and Context

Unsure that freud works on more platforms.

## How Has This Been Tested?

CI checks.

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [ ] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `194c5bc27976`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.