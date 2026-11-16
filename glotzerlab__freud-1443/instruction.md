# Issue

**Title:** Remove garnett support and corresponding documentation

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
Support for garnett frames has been removed from the locality module (the only place where garnett remained), and all mentions of garnett have been removed from the documentation.

## Motivation and Context
Garnett is not actively maintained, and its repository has been archived. This PR  .

## How Has This Been Tested?
There are no tests involving garnett objects, so no changes to testing are needed.

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [x] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `311984b294a4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.