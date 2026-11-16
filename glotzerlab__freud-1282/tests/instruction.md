# Issue

**Title:** Nanobind port: Cluster module

## Description

## Description
Translate the Cluster module from cython to nanobind.

## Motivation and Context
One component of the push to translate the freud from cython to nanobind.

## How Has This Been Tested?
The cluster tests were not modified, but now all pass.

## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
<!-- Put an `x` in all the boxes that apply. If you're unsure about any of
     these, don't hesitate to ask. We're here to help! -->
- [x] I have read the [**CONTRIBUTING**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md) document.
- [x] My code follows the code style of this project.
- [ ] I have updated the documentation (if relevant).
- [x] I have added tests that cover my changes (if relevant).
- [x] All new and existing tests passed.
- [ ] I have updated the [credits](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst).
- [ ] I have updated the [Changelog](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `70ee09bc6175`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.