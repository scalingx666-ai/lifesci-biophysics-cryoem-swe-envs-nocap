# Issue

**Title:** Make the error optional for plot_convergence

## Description

Currently the plot_convergence demand the input dataframe to have forward_error and backward_error. This PR makes these two input optional. So when these two columns are not present in the input dataframe, no errorbar will be plotted.

I have not touched the array input as we will deprecate them in #247.

A final_error keyword has been added to deal with the case where error is not provided and the user wish to plot the error of the final estimate.

Fix the issue where when too many data points are included, the x-axis will be too crowded.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3c59131435b3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.