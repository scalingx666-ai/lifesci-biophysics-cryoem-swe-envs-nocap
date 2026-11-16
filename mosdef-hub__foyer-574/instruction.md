# Issue

**Title:** Replace `pkg_resources` with `importlib` and add python 3.13 to test suite, bump numpy to 2.0

## Description

### PR Summary:

I'm trying to fix some of the deprecation errors we're getting in the mosdef packages. One of them is coming from foyer with the use of `pkg_resources`. 

[pkg_resources](https://setuptools.pypa.io/en/latest/pkg_resources.html) will be removed in python 3.13

Note: This PR is not compatible with python 3.9, so we can't merge it until we want to drop 3.9 support and add 3.13.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `07e5f235a9e5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.