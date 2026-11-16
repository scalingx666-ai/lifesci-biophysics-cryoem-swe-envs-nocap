# Issue

**Title:** Pin OpenMM to <8.4 to fix failing CI

## Description

### PR Summary:

The CI is failing with OpenMM 8.4. Until we can dig into the root cause, we should pin OpenMM to <8.4 in the conda .yml files.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `12df69184c7b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.