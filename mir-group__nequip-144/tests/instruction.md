# Issue

**Title:** Output arbitrary fields to XYZ from `nequip-evaluate`

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
Option `--output-fields` to allow outputting arbitrary fields.

## How Has This Been Tested?
Ran it, adding a test.

## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [ ] Bug fix (non-breaking change which fixes an issue)
- [X] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
<!-- Put an `x` in all the boxes that apply. If you're unsure about any of
     these, don't hesitate to ask. We're here to help! -->
- [X] My code follows the code style of this project and has been formatted using `black`.
- [X] All new and existing tests passed, including on GPU (if relevant).
- [ ] I have added tests that cover my changes (if relevant).
- [X] The option documentation (`docs/options`) has been updated with new or changed options.
- [X] I have updated `CHANGELOG.md`.
- [ ] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c6b3d76b44fc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.