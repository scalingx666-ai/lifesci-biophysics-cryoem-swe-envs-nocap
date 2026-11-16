# Issue

**Title:** Add validation for orientations when calculating bond order diagrams

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
The non-default modes for bond order diagram calculation are no longer supported when the user does not explicitly provide particle orientations.

## Motivation and Context
Previously, the lack of validation on provided orientations produced unexpected behavior when `mode` was not "bod", which led to confusion among users even though the documentation implies that orientations should be given for other modes. This PR  .

## How Has This Been Tested?
All of the existing tests provide orientations, so this change should not affect test behvaior.

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [x] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `16420ab7ad52`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.