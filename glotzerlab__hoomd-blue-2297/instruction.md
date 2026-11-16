# Issue

**Title:** Additional mesh exclusions

## Description

## Description

I added meshbond exclusion to the neighbor lists, which not only excludes bonded particles in the mesh but also particles that are separated by an edge. This should make dynamical bonding more stable for large mesh deformations and fix the error that pair potentials are not considered in the flip. 

Secondly, I tied up the MeshGroupData class. 

## How has this been tested?

Updated pytests and tested exclusions on Hodges.

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8afe7d6e049d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.