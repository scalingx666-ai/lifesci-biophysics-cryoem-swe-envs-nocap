# Issue

**Title:** Add workaround for scipy.special.factorial2 changes

## Description

This PR adds a `factorial2` function that wraps `scipy.special.factorial2` to return 1.0 when the input is not positive. This is a temporary addition, while we wait for Scipy's update. To learn more, see  This is a better solution than limiting Scipy version that works with IOData, as one might need to use scipy `1.11.x` in their environment for other purposes. A similar change was made in other QC-Devs packages; see

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ca6113b60fc9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.