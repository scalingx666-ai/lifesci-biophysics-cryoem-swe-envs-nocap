# Issue

**Title:** Fix division by zero in mpcd collision

## Description

## Description

In the thermalize rigid body particles step, there is a potential division by zero if a rigid body has particles that have zero mass.

## Motivation and context

This allows for rigid bodies to have constituent particles that do not participate in the MPCD collision step and have zero mass.

 

## How has this been tested?

A pytest has been added to check for this error to future proof the code.

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b302336c83fa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.