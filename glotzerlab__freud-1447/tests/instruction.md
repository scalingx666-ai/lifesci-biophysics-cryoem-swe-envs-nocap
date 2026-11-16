# Issue

**Title:** Default angular separation

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
Added $(-1, 0, 0, 0)$ as a default equivalent quaternion to `AngularSeparationNeighbor`, `AngularSeparationGlobal`, and `LocalBondProjection`, in response to an open issue.

## Motivation and Context
The quaternions  $(1, 0, 0, 0)$ and $(-1, 0, 0, 0)$ both represent the identity, and so in all physical applications should both be used.

Resolves: #1241

## How Has This Been Tested?
 Added a test, `test_compute_defaults` for `AngularSeparationGlobal` in `test_environment_angular_separation.py` to ensure the default value provides the correct angular displacement.

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [x] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `16420ab7ad52`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.