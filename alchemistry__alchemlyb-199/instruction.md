# Issue

**Title:** Sync series and dataframe in statistical_inefficieny()

## Description

This adds a number of tests for slicing() and statistical_inefficieny():
* Test that slicing() respects upper and lower time bounds (currently
  passes)
* Test that statistical_inefficieny() respects upper and lower time bounds
  when it is used without series to subsample (currently passes)
* Test that statistical_inefficieny() respects upper and lower time bounds
  when it is used with series to subsample (currently fails)
* Test that first using slicing() on the data frame, then
  statistical_inefficieny() without time bounds yields the same results as
  a single call to statistical_inefficieny() with time bounds (currently
  fails)

It then makes sure that both the subsampling series and the dataframe are sliced
when calling statistical_inefficieny(). This ensure that the series and
the dataframe are in sync before the subsampled indices of the series
are applied to the dataframe. This fixes the above tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72622c99c16c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.