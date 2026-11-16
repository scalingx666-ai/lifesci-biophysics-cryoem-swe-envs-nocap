# Issue

**Title:** Fix msd

## Description

MSD is broken. 

## Description
There seem to be two issues with it - the incorrect procedure for unwrapping and it seems that wrap function was used instead of unwrap. We didn't have tests that could catch any of this.
## Motivation and Context
Resolves: #1342 

## How Has This Been Tested? 
I added a test based on MWE from the issue to check if compute function runs correctly.


## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [x] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3f0dc2d111a3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.