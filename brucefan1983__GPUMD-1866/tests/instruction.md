# Issue

**Title:** Allow to use flexible ZBL for normal NEP only

## Description

### Summary

Restrict flexible ZBL to ordinary NEP (`model nep`).

### Changes

- Enforce the restriction in training, model loading, and format conversion.
- Explicitly reject ZBL settings for dipole and polarizability models.
- Remove unused flexible ZBL parameters and kernel branches from charge and vdW MD implementations.
- Update input and model-format documentation.

### Validation

- Full regression suite: 255 passed; all 25 cross-case relations passed.
- Full pytest suite: 745 passed, 482 skipped, 0 failed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4b5b7850412a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.