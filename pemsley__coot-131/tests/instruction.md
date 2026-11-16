# Issue

**Title:** Fix problem if spline with 1 control point is requested.

## Description

Do not crash if spline is requested with only 1 control point - this happens drawing ribbons in e.g. 4v6x. Arguably this could be trapped higher in the chain, but fix here is less intrusive.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f908f0582304`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.