# Issue

**Title:** Drop python 3.10 and add python 3.14 to testing

## Description

Similar to foyer and gmso, we can start updating the python coverage in mBuild as well. There is one small fix for a deprecation in numpy, and I bumped up the versions of the various actions in the CI.

### PR Summary:

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f9e7c5be31a9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.