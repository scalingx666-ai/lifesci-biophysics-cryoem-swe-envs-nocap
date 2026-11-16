# Issue

**Title:** Fix to external periodic field issue with 2D systems

## Description

## Description

When trying to use external periodic fields in 2D systems, there is an error caused by a division by zero. The changes add a proper way to compute the reciprocal vectors in 2D cases, solving the error.

## Motivation and context

Currently the function to apply periodic external fields is blocked for 2D systems due to an error described in the issue #1475 . The fix removes the error message thrown when the system is 2D, and adds the definition of the proper reciprocal vectors for 2D.

.

## How has this been tested?

The fix was tested using regular Lennard-Jones particles and rigid bodies. The external field is applied in x- and y-directions. The error message described in #1475 no longer appears.

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x ] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `847852111095`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.