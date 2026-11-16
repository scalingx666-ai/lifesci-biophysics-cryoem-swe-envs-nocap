# Issue

**Title:** Make it that the MBAR will take in maximum_iterations argument.

## Description

Currently, at least `maximum_iterations` is not really being used during the MBAR solver.
So I changed the MBAR to make sure that it is taken into account as is suggested in 

Note the doc says other things though https://pymbar.readthedocs.io/en/master/mbar.html.

I cannot write a test to test this as `pymbar.mbar.MBAR` doesn't have a public attribute that echoes this assignment.

It is hard to raise an issue either as the bug will show itself when MBAR didn't converge within the default 250 iterations and most of the calculations converge in less than 250 iterations.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7af608c17061`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.