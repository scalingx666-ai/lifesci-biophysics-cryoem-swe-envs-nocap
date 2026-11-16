# Issue

**Title:** Warn when the covariance-weighted fit disagrees with ordinary least squares

## Description

# Warn when the covariance-weighted fit disagrees with ordinary least squares

## The problem

`bayesian_regression` weights the observations by the inverse of an estimated covariance matrix.
That matrix is estimated from a finite trajectory, and at long lag times it rests on very few
independent samples. Occasionally sampling noise is enough to make it numerically singular.

When that happens, the inversion returns a gradient in error by up to three orders of magnitude -
and nothing in the output indicates it. There is no exception, no failed convergence, and the
posterior interval is narrow. The diffusion coefficient is simply wrong, and looks plausible.

Across 64,000 fits of a lattice random walk whose true diffusion coefficient is unity by
construction, this occurred 14 times, a rate of roughly one in five thousand. The affected
mean-squared displacement curves are not distinguishable by inspection from healthy ones.

## Why a diagnostic on the covariance does not work

The obvious approach is to inspect the matrix and refuse to proceed if it looks bad. I tested five
spectral summaries on the population above - condition number, smallest eigenvalue, number of
negative eigenvalues, negative fraction, and the condition number of the positive subspace - using
a threshold fitted on three system sizes and applied to a fourth.

None of them separates the affected fits from the rest. The most negative eigenvalue observed
anywhere in the population belongs to a fit that returned a **correct** answer, and one affected fit
fails carrying a single negative eigenvalue while healthy matrices routinely carry more than twenty.

## What this PR does

It compares the fitted gradient against an ordinary least squares estimate of the same data, and
warns when they disagree by more than a set factor.

The ordinary least squares fit does not use the covariance, so it cannot be corrupted in the same
way. It is a less precise estimator but a robust one, and the disagreement between the two is
evidence about the covariance rather than about the data.

**The OLS slope is already computed.** `bayesian_regression` calls `linregress` to seed the MCMC
walkers and then discards the result. This PR keeps it. There is no additional computation and no
measurable cost.

On 400 healthy fits spanning four system sizes, the check raises a false alarm 1.5% of the time at
the default threshold. It identifies every covariance-driven failure in the test population.

## Interface

The check runs by default and warns rather than raising, so existing code is unaffected except for
the warning itself.

```python
diff.bayesian_regression(start_dt)                          # check on, warns if triggered
diff.bayesian_regression(start_dt, check_consistency=False) # off
diff.bayesian_regression(start_dt, warn_threshold=0.5)      # less sensitive

diff.consistency_ratio     # the ratio itself, for programmatic use
```

A triggered warning reads:

```
The fitted gradient is 0.412 times the ordinary least squares estimate. A ratio this low
indicates that the covariance matrix may be numerically singular and that the diffusion
coefficient may be unreliable. Consider refitting with recondition=True, or inspecting the
eigenvalues of Diffusion.covariance_matrix.
```

## Choice of threshold

On the test population the healthy fits have a minimum ratio of 0.623 and the affected fits a
maximum of 0.604, so any threshold in that interval separates them perfectly on this data. A
threshold of 0.7 is used instead.

The interval is only 0.019 wide and is defined by the single most extreme observation in each
group. A threshold placed inside it would be fitted to the sample rather than justified by it, and
could not be expected to hold on unseen data. 0.7 sits clear of the healthy minimum by a margin
comparable to the scatter of the healthy population, at a cost of 1.5% false alarms. For a check
whose purpose is to warn, an occasional unnecessary warning is preferable to a silent miss.

## Limitations

**It does not detect non-diffusive trajectories.** One of the fourteen affected cases has a
mean-squared displacement that departs visibly from linearity. Its OLS slope is depressed for the
same reason its Bayesian estimate is, so the two agree and the check does not fire. That case is
not a covariance failure, and the check is not designed to catch it. The figure quoted above is
therefore all 13 covariance-driven failures, not all 14 anomalies.

**The threshold is calibrated on simulated data** where the true answer is known. It has not been
validated against real molecular dynamics trajectories at scale, since no ground truth exists there.

**It warns; it does not correct.** Reconditioning remains the remedy.

## Testing

Six tests are included: that the ratio is near unity on healthy data, that no warning fires on
healthy data, that a suppressed gradient triggers a warning, that the check can be disabled, that
the threshold is respected, and that calling the check before a regression is safe.

## Provenance

This arose from an MSc dissertation characterising the failure, supervised by @arm61. Full
analysis, the 64,000-fit population study, and the diagnostic evaluation are at
https://github.com/ShubhamX57/kappa-kinisi.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4828d06d12cd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.