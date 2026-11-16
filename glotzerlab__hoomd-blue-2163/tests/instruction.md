# Issue

**Title:** Export‑name validation and conflict resolution for FreeVolume

## Description

<!-- Please confirm that your work is based on the correct branch. -->
<!-- Bug fixes should be based on *trunk-patch*. -->
<!-- Backwards compatible new features should be based on *trunk-minor*. -->
<!-- Incompatible API changes should be based on *trunk-major*. -->

## Description

<!-- Describe your changes in detail. -->
This PR adds a unit test checking that the export name of `FreeVolume` compute matches the integrator class name, and resolves an existing export‑name conflict for the `FreeVolume` class in the HPMC module.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
<!-- Replace ??? with the issue number that this pull request resolves. -->
Discussed in #2160


## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
A variation on `test_shape_attached` that added `compute.FreeVolume` tests that all C++ export names are correct.

<!--- Please build the sphinx documentation and check that any changes to
      documentation display properly. -->

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk-minor/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk-minor/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6df3e7d4884b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.