# Issue

**Title:** TI with gaussian quadrature

## Description

Added an optional gaussian quadrature functionality to estimate the TI integral. 

This functionality could be triggered when initializing: `TI(gauss_qua=True)`

The lambda values and weights were referred the Amber manual (page 493): http://ambermd.org/doc12/Amber22.pdf

The formula used to estimate the free energy and standard errors were referred the the Amber manual (eq. 25.2) and this literature (eq. 3): https://pubs.acs.org/doi/10.1021/acs.jcim.2c01052?ref=pdf, respectively.

The final two dataframes with free energy values and errors are upper triangluar matrix, because the values are cumulative sum upto each lambda state rather than differences. The diagonal values are the estimation at each lambda.

I have tested 12 lambdas and 9 lambdas with Amber one-step method to estimate the free energy and error of each leg in the thermodynamic cycle. The results are comparable to the ones estimated with trapezoidal rule. The other number of lambdas (1, 2, 3, 5, 7) should work as well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f5cf43a31325`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.