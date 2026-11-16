# Issue

**Title:** Add `pre-commit` hook + Add black to CI

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
I started using nequip recently with great results. 
This PR does not include any changes to the Code but adds:
-  a [pre-commit](https://pre-commit.com/) hook.
-  updated CI to test for black / flake8.
- testing black and flake8 now uses an image.
-  `.flake8` config file based on `CONTRIBUTING.md`.



## Motivation and Context
I found myself adding these when looking into the nequip code and wanted to share them.

## How Has This Been Tested?
I use a similar setup myself. The only difference is that I'm also using the https://github.com/PyCQA/isort package in addition.

## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [x] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
<!-- Put an `x` in all the boxes that apply. If you're unsure about any of
     these, don't hesitate to ask. We're here to help! -->
- [x] My code follows the code style of this project and has been formatted using `black`.
- [x] All new and existing tests passed, including on GPU (if relevant).
- [x] I have added tests that cover my changes (if relevant).
- [x] The option documentation (`docs/options`) has been updated with new or changed options.
- [ ] I have updated `CHANGELOG.md`.
- [ ] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c5f36b03052e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.