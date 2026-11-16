# Issue

**Title:** Fix: Correct alpha parameter implementation in PPPM (line 137)

## Description

<!-- Please confirm that your work is based on the correct branch. -->
<!-- Bug fixes should be based on *trunk-patch*. -->
<!-- Backwards compatible new features should be based on *trunk-minor*. -->
<!-- Incompatible API changes should be based on *trunk-major*. -->

## Description

Fix the alpha parameter implementation in PPPM method (`hoomd/md/long_range/pppm.py`, line 137).

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
Fixes an issue where the PPPM always behaved as if `alpha=0`, regardless of the value set in `hoomd.md.long_range.pppm.make_pppm_coulomb_forces()`. The update ensures that the specified `alpha` parameter is correctly passed and applied during PPPM force construction.



## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->

<!--- Please build the sphinx documentation and check that any changes to
      documentation display properly. -->

Using the same testing code as in #2152, setting `alpha` to non-zero values now produces distinct energy curves—confirming that the parameter is correctly applied.

## Checklist:

- [X] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/hoomd-blue/blob/trunk-minor/CONTRIBUTING.rst).
- [X] I agree with the terms of the [**HOOMD-blue Contributor Agreement**](https://github.com/glotzerlab/hoomd-blue/blob/trunk-minor/ContributorAgreement.md).
- [ ] My name is on the list of contributors (`sphinx-doc/credits.rst`) in the pull request source branch.
- [x] I have summarized these changes in `CHANGELOG.rst` following the established format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ef9d575b8d34`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.