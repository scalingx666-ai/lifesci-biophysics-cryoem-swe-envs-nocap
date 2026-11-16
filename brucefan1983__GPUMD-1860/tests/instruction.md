# Issue

**Title:** Add qnep3 model

## Description

### Summary

Add a new charge model, `qnep3`, with the reciprocal-space charge contribution and the self-energy correction. Preserve the existing behavior of `qnep1`, `qnep2`, and `qnep3_vdw`.

### Changes

- Add `model qnep3` as the user-facing model selection; keep the legacy `charge_mode` keyword restricted to `0/1/2`.
- Apply the self-energy correction only for internal charge mode 3, after the long-range calculation and before subtracting the mean of `D_real`.
- Use the charge-neutralized charges consistently in training and MD, allowing the correction to enter forces and virials through the existing descriptor chain rule.
- Update input/model-format documentation and add model-consistency tests for `qnep3`; do not extend replica support.


### Validation

- Full regression suite: 255 passed; all 25 cross-case relations passed.
- Full pytest suite: 745 passed, 482 skipped, 0 failed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa6c96a975d5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.