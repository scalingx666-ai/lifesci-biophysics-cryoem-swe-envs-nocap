# Issue

**Title:** Drop Testing for py38 in Windows

## Description

### PR Summary:
We again bumped into unknown error for windows py3.8 test, probably similar to that seen windows py3.9. This PR will temporarily drop the test for windows py3.8 CI run. 

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `28b7c530214e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.