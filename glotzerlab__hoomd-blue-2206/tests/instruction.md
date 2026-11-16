# Issue

**Title:** Fix mpcd particle positions

## Description

## Description

Fix bug in mpcd pytest with rigid bodies in which the mpcd particles are not placed in the same cell as the constituent particles.

## Motivation and context

The mpcd pytests that check momentum conservation when colliding with rigid body constituent particles require that mpcd particles be placed in the same location as the constituent particles to ensure a collision occurs. When testing periodic boundaries by placing the rigid body in a corner of the simulation box, the mpcd particles were not placed in the right location.

## How has this been tested?

The positions in the test have been checked manually to ensure they are now in the right place.

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [ ] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4de0412e8c70`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.