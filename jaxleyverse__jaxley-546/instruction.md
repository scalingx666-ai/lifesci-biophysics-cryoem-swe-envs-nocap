# Issue

**Title:** Update Regression CI

## Description

Since maintaining a baseline is not possible ( for context), I have modified the regression CI to run baselines on main and tests on the PR branch in one go. The trigger for this is now a comment with "/test_regression".
This also makes skipping the regression tests automatic when there is no baseline_file around, rather than having to add `pytest -m "not regression"` every time.

Reuses a lot of code from before. Works for the moment. Long term we could look at using `asv` instead. ()

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a6c19ab2e74d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.