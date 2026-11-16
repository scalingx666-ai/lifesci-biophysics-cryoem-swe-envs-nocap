# Issue

**Title:** Create alchemy subpackage

## Description

## Description
The idea of this PR is to create the alchemy subpackage in a way that doesn't break the current API.

This is to give room to extend the `alchemy` capabilities of openmmtools with the tools that would be coming from `perses` and `openfe` (for example check roadmap in https://github.com/orgs/choderalab/projects/5)

In the future, I expect that these objects will be moved around to different modules or that the current `alchemy.py` module changes its name for something more descriptive of what it has. So far, this is just thought as a starting point.

## Todos
- [x] Implement feature / fix bug

This shouldn't be changing any of the behavior, this should be reflected in the tests in `test_sampling.py` and `test_alchemy.py` passing, which are the ones that call the objects in the `alchemy` module.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0b4d42a3b898`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.