# Issue

**Title:** Improve the error handling in the backward and forward convergence

## Description

In the backward and forward convergence, for the initial set of points, which uses for example the first 10% of the date, it could be the cases where due to the fact that there are not many data points, so the overlap is pretty bad, which gives terrible statistical error.
This PR is build on 
The PR allow the user to specify a error tolerance. If the statistical error is too bad, use the bootstrap error instead

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `46cc83b49a20`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.