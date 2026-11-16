# Issue

**Title:** Add 1-2 exclusions to hard-sphere random walk.

## Description

This PR adds 1-2 (directly bonded) pair exclusions in the overlap check. This is needed to allow bond lengths less than the overlap diameter. Excluded pair indices are passed into the numbajit check_path method.

This also checks any given angles sampler against cases where combination of max angle, radius and bond length would result in a significant amount of failed attempts, since we don't have 1-3 exclusions (I'm not sure if we really need them). Finally, I added a few useful (and easier to read) attributes for common checks against RandomWalkState.


### PR Checklist
------------
 - [x] Includes appropriate unit test(s)
 - [x] Appropriate docstring(s) are added/updated
 - [x] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `09e2cdb32d7e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.