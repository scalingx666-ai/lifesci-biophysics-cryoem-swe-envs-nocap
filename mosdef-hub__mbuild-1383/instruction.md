# Issue

**Title:** bug fix in volume constraint buffer value passed to Constraint plugin.

## Description

Use particle size for the volume constraint buffer value in the main loop instead of tolerance

### PR Summary:

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `54f30757212c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.