# Issue

**Title:** remove sklearn dependence

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
remove the sklearn package and use `torch.solve` instead.

## Motivation and Context
Make the module more robust to multicomponent and large dataset.

## How Has This Been Tested?
Test set is updated and use stricter criteria

## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [x] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
<!-- Put an `x` in all the boxes that apply. If you're unsure about any of
     these, don't hesitate to ask. We're here to help! -->
- [x] My code follows the code style of this project and has been formatted using `black`.
- [x] All new and existing tests passed, including on GPU (if relevant).
- [x] I have added tests that cover my changes (if relevant).
- [x] The option documentation (`docs/options`) has been updated with new or changed options.
- [x] I have updated `CHANGELOG.md`.
- [x] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0f3909161d38`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.