# Issue

**Title:** Clean up method parameters in `build.py` so default behavior is consistent.

## Description

### PR Summary:

Some build methods had a way to handle `path=None` while others left `path` as a required parameter. This changes it so all methods in `build.py` defualt to `path=None` and create a new Path instances. 

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `79deb0797bb3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.