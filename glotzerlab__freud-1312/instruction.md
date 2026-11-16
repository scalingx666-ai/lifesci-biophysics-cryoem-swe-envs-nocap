# Issue

**Title:** Add CIF file reader based on Parsnip

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description

Many users wish to initialize crystals from CIF files, but encounter convention mismatches when converting between ASE and HOOMD scripts. Parsnip provides a lightweight alternative that has been tested against the Freud convention for accuracy.

~NOTE: parsnip is not yet on conda-forge, meaning the current environment file will not work. I can install with pip, but we could also just wait a few days for the conga-forge feedstock to be merged.~ 


## How Has This Been Tested?
New tests have been added in the style of other `freud/data.py` tests. There is additional testing on the Parsnip side, which I would be happy to expand on if we feel it necessary - I test the creation and initialization of data from a variety of CIF files, but I do not have tests for parsnip->HOOMD directly.

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [ ] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `920700a38199`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.