# Issue

**Title:** add mapped keys to include keys for ase dataset

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
<!-- Describe your changes in detail. -->
In the NPZ dataset, the keys in `key_mapping` are automatically added to the include_keys. But it is not the case in the case dataset. 

## Motivation and Context
The from_ase function is slightly modified to make the ase dataset and npz dataset perform consistently.

## How Has This Been Tested?
<!-- Please describe in detail how you tested your changes. -->
<!-- Include details of your testing environment, and the tests you ran to
     see how your changes affect other areas of the code, etc. -->

## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
<!-- Put an `x` in all the boxes that apply. If you're unsure about any of
     these, don't hesitate to ask. We're here to help! -->
- [ ] My code follows the code style of this project and has been formatted using `black`.
- [ ] All new and existing tests passed, including on GPU (if relevant).
- [ ] I have added tests that cover my changes (if relevant).
- [ ] The option documentation (`docs/options`) has been updated with new or changed options.
- [ ] I have updated `CHANGELOG.md`.
- [ ] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a90d22d83f43`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.