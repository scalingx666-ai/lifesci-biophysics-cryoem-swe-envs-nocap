# Issue

**Title:** Always flush gsd writers on exit

## Description

## Description

<!-- Describe your changes in detail. -->
Use `weakref.finalize` to flush GSD files on exit.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
Apparently, it is a "feature" that Python does not always clean up objects when garbage collecting on exit. It does 95% of the time, but certain user scripts trigger cases where it does not. HOOMD-blue relies on C++ destructors to flush and close GSD files cleanly. Thus, we need to use `weakref.finalize` to ensure that all GSD files are flushed on exit.

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
The test case in #2234 passes.

<!--- Please build the sphinx documentation and check that any changes to
      documentation display properly. -->

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd16aa70c05d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.