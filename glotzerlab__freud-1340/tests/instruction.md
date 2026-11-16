# Issue

**Title:** Build wheels for Windows

## Description

## Description
Trying to make the wheels for Windows using the progress made in #749 

## Motivation and Context
delvewheel [has improved](https://github.com/adang1345/delvewheel/blob/master/CHANGELOG.md#180-16-august-2024) over the four year this was lasty tried so want to see if it possible now.
Resolves: #1336

## How Has This Been Tested?
Renabling the CI test for building Windows while disabling the ones in mac and ubuntu for the moment.

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [ ] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f0908eb55fe4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.