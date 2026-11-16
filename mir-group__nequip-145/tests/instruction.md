# Issue

**Title:** :bug: bugfix to match the order between graph_selector and node selector in dataset statistics

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description
<!-- Describe your changes in detail. -->

## Motivation and Context
<!-- Why is this change required? What problem does it solve? -->
<!-- Replace ??? with the issue number that this pull request resolves, if applicable. -->
When subdataset is used (`self._indices is not None`), Dataset Statistics will sample configurations from the subdataset instead of the whole dataset. However, the graph_selector follows the same order in `self._indices` while the node_selector sorts the graph_selector and then augments to the node. That's why they have different orders.

This will lead to wrong `per_species` statistics.

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
- [x] All new and existing tests passed, including on GPU (if relevant).
- [x] I have added tests that cover my changes (if relevant).
- [ ] The option documentation (`docs/options`) has been updated with new or changed options.
- [ ] I have updated `CHANGELOG.md`.
- [ ] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `62996aca6fbb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.