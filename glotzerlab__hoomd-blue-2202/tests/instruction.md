# Issue

**Title:** Dynamical bonding check3

## Description

## Description

The new check fixes a bug of possible duplicated bonds and dangling vertices in the dynamical bonding code. The code now checks at every flip attempt whether both vertices have more than 3 neighbors. 

## Motivation and context

The new code fixes the bug and avoids crashes when using the Helfrich potential

## How has this been tested?

Stress tested at various meshes, and all tests still work properly.

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2729d3a092b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.