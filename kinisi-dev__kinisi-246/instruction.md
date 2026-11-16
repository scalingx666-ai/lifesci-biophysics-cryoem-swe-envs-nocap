# Issue

**Title:** Replace the GLS/OLS ratio check with a Hausman test

## Description

The consistency check after `bayesian_regression` compared the ratio of the GLS and OLS gradients with a fixed threshold. It warned only on the low side, returned silently when the OLS gradient was not positive, and its threshold did not scale with the noise in the data. This replaces it with a Hausman-type test.

## The statistic

With C the covariance matrix used in the fit (over the time intervals from `diff_regime` on) and design matrix X = [dt, 1] (X = [dt] when `fit_intercept=False`):

- Var_GLS = [(X^T C^-1 X)^-1]_00, with the same `pinvh` as the likelihood;
- Var_OLS = [(X^T X)^-1 X^T C X (X^T X)^-1]_00;
- z = (g_GLS - g_OLS) / sqrt(Var_OLS - Var_GLS).

g_GLS is the analytic GLS estimate, (X^T C^-1 X)^-1 X^T C^-1 y, not the MCMC median. It is deterministic, it is the quantity Var_GLS describes, and, unlike the MCMC median, it is not pushed upwards when the gradient is close to zero by the likelihood rejecting negative gradients. Since it needs no samples, the check now runs before the MCMC.

## Warnings

A `UserWarning` is raised when:

- |z| > `z_threshold` (default 3), in either direction;
- the OLS gradient is zero or negative (previously the check returned silently);
- z is not finite;
- the covariance matrix is too close to singular for its pseudo-inverse: Var_OLS - Var_GLS <= 0, Var_GLS <= 0, or X^T C^-1 X cannot be inverted (Var_GLS is then taken as infinite, so the check cannot stop the fit). z is then reported as NaN.

## API

- `warn_threshold` is replaced by `z_threshold` on `Diffusion.bayesian_regression`. `warn_threshold` was added after 2.1.0 and never released, so there is no deprecation path.
- New property `Diffusion.consistency_z`. `Diffusion.consistency_ratio` is kept for information. Both are `None` before a regression and when `check_consistency=False`; they are reset at the start of each regression, so a disabled check does not leave values from an earlier fit.

## Structure

The computation and the decision are separate private functions, `_gradient_estimates(abscissa, ordinate, covariance, fit_intercept)` and `_check_consistency(gls_gradient, ols_gradient, gls_variance, ols_variance, z_threshold)`, so each can be tested directly on small hand-built inputs. The method `Diffusion._check_consistency` is removed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3b04d958cfed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.