# Issue

**Title:** modify to_ase

## Description

<!-- Provide a general summary of your changes in the Title above. -->
extend TestTimeXYZFileWriter and modify to_ase to handle tensor. 

## Description
<!-- Describe your changes in detail. -->
Now `TestTimeXYZFileWriter` can accept user-defined `extra_fields` argument.
Previously, it couldn't handle 3x3 shape tensor for node feature. (e.g. BEC)
Now, `to_ase` can handle this problem. 

## Motivation and Context
<!-- Why is this change required? What problem does it solve? -->
<!-- Replace ??? with the issue number that this pull request resolves, if applicable. -->


## How Has This Been Tested?
<!-- Please describe in detail how you tested your changes. -->
<!-- Include details of your testing environment, and the tests you ran to
     see how your changes affect other areas of the code, etc. -->
I tested with `Nequip-LES` to print out 'LES_q' and 'LES_BEC' using `TestTimeXYZFileWriter`. 
This works perfectly fine. 

## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [x] Bug fix (non-breaking change which fixes an issue)
- [x] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
<!-- Put an `x` in all the boxes that apply. If you're unsure about any of
     these, don't hesitate to ask. We're here to help! -->
- [x] I have read ["Contributing to NequIP"](../docs/dev/contributing.md).
- [x] My code follows the code style of this project and has been formatted using `black`.
- [ ] All new and existing tests passed, including on GPU (if relevant).
- [ ] I have added tests that cover my changes (if relevant).
- [ ] I have updated `CHANGELOG.md`.
- [ ] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `00c46b733024`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.