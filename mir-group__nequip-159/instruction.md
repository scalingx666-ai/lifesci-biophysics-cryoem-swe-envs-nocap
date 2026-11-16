# Issue

**Title:** Updated example.yaml + full.yaml with better defaults + comments

## Description

Updated example.yaml + full.yaml with better defaults + comments

## Description

Change metrics defaults to total F-MAE/F-RMSE, larger max_epochs and provide better comments. 

## Motivation and Context

* Old comments had still proposed a 100:1 F:E weighting
* Old metrics were per-species F-MAE

## Types of changes
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds or improves functionality)
- [x] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
- [ ] My code follows the code style of this project and has been formatted using `black`.
- [ ] All new and existing tests passed, including on GPU (if relevant).
- [ ] I have added tests that cover my changes (if relevant).
- [ ] The option documentation (`docs/options`) has been updated with new or changed options.
- [x ] I have updated `CHANGELOG.md`.
- [ x] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `96fdebfaf5e7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.