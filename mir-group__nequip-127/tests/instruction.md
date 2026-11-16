# Issue

**Title:** Log git commit for builders

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
Log the git commit of any modules from which builders are taken.

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
- [X] All new and existing tests passed.
- [X] I have added tests that cover my changes (if relevant).
- [X] The option documentation (`docs/options`) has been updated with new or changed options.
- [X] I have updated `CHANGELOG.md`.
- [X] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `abd49171a281`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.