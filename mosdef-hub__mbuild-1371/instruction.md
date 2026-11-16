# Issue

**Title:** Add carbon nanotube builder to lib/recipes

## Description

### PR Summary:
I've been using an mBuild-based CNT builder from https://github.com/whitehml/Nanotube-Builder for a while. I figured it would be worth incorporating this into a built in recipe, making it much more visible and accessible. This PR adopts the recipe from https://github.com/whitehml/Nanotube-Builder, fixes several deprecation warnings, and includes some improvements to the generation algorithm.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f5e096049ee6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.