# Issue

**Title:** Nanobind Cluster

## Description

Added cluster module to pre-commit, build_wheels, and run_tests.

## Description
Added cluster module to pre-commit, build_wheels, and run_tests.

## Motivation and Context
Ensures github runs tests for cluster module.

## How Has This Been Tested?
Ran pre-commit locally before pushing.

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
- [x] I have updated the documentation (if relevant).
- [x] I have added tests that cover my changes (if relevant).
- [ ] All new and existing tests passed.
- [x] I have updated the [credits](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst).
- [ ] I have updated the [Changelog](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8717edf8d037`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.