# Issue

**Title:** Add testing for python 3.11

## Description

### PR Summary:
Drop CI testing for python 3.8 and add test for 3.11 to be in line with mbuild and gmso. Also attempt to remove the `examples` per 

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ed5ccb4429e1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.