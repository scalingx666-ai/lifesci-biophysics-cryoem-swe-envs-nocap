# Issue

**Title:** Avoid pandas df.iterrows dtype conversion

## Description

Super small fix: the pandas df.iterrows() method reverts ints to floats, but in this case the results are used as list indices and must remain ints. This was causing exceptions/errors for me when using k-fold cross validation. See the Note at https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.iterrows.html

Replaced with df.itertuples() which preserves dtypes (as per suggestion in that same Note).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ba91e19e3452`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.