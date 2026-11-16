# Issue

**Title:** Optimize the fit gradients

## Description

- Save the unrotated positions. This can avoid rotating the positions again in `calc_fit_forces_impl`;
- Determine dl, dq or ds by compile-time template options;
- Only call calc_fit_gradients when atom groups have explicit gradients;
- Simplify and inline `cvm::quaternion::position_derivative_inner`.

This PR should be only marked as ready after #713 is merged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7915bd23bcfc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.