# Issue

**Title:** Improve the sampling of rotation trial moves.

## Description

## Description

<!-- Describe your changes in detail. -->
Implement the rotation move algorithm from [10.1016/j.jmgm.2006.04.002](https://doi.org/10.1016/j.jmgm.2006.04.002). This is a breaking change because it redefines the the rotation trial move size `a`.

This code was originally written in Rust by @janbridley (). I converted it to C++ for use with HOOMD-blue.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->
The new distribution well-defined and behaves better than the old.

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
This plot compares the distribution of rotation angles that result from the rotation moves.
![rotate-move-compare](https://github.com/user-attachments/assets/4a713aa9-2ec4-40dc-8363-aaf93e7eca2b)

<!--- Please build the sphinx documentation and check that any changes to
      documentation display properly. -->

## Checklist:

- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/CONTRIBUTING.rst).
- [x] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk/ContributorAgreement.md).
- [x] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e0a8f6766695`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.