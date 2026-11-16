# Issue

**Title:** Logging correct Helfrich energy

## Description

## Description

The Helfrich energy has been assigned to each vertex for each outgoing mesh bond, making it roughly 6 times larger. 

## Motivation and context

Calculating energy correctly for logging purposes. 

## How has this been tested?

Running simulations. 

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `059d81082b22`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.