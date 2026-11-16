# Issue

**Title:** Add rng as passable parameter to Constraint

## Description

### PR Summary:

`sample_candidates` in the Constraint classes uses a seed, and this is never set from the rng state, causing 2 random walks to have different coordinates when a volume constraint is passed in. This wasn't caught by the tests because it only arises when passing in a volume constraint without an initial_point (which then calls `sample_candidates`).

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `194fcb232657`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.