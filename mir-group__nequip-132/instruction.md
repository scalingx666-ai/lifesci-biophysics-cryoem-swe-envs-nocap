# Issue

**Title:** add conditions in converting data to torch.Tensor for AtomicData

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
Current data conversion cannot cover the case where the values are from fixed fields and are bool or list. It will throw an error, for example, when fixed fields include `pbc: true`.

## Motivation and Context
<!-- Why is this change required? What problem does it solve? -->
<!-- Replace ??? with the issue number that this pull request resolves, if applicable. -->
Resolves: #???

## How Has This Been Tested?
<!-- Please describe in detail how you tested your changes. -->
<!-- Include details of your testing environment, and the tests you ran to
     see how your changes affect other areas of the code, etc. -->

## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [x] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
<!-- Put an `x` in all the boxes that apply. If you're unsure about any of
     these, don't hesitate to ask. We're here to help! -->
- [ ] My code follows the code style of this project and has been formatted using `black`.
- [ ] All new and existing tests passed.
- [ ] I have added tests that cover my changes (if relevant).
- [ ] The option documentation (`docs/options`) has been updated with new or changed options.
- [ ] I have updated `CHANGELOG.md`.
- [ ] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb33114be633`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.