# Issue

**Title:** Fix vmd metadynamics errors

## Description

In metadynamics, the vector of Gaussian sigmas is required: if not defined from user input, it remains of size 0 but it is never used because an error is raised.  However, in VMD specifically errors are non fatal and initialization would continue, triggering a segfault. This PR fixes this, while also providing the required keyword in the (currently broken) Dashboard template.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e552ada4745f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.