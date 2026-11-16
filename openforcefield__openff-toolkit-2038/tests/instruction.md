# Issue

**Title:** [WIP] Re-ignore external examples in conda tests

## Description

- [x]  

This will be auto-fixed when we cut a release, but I don't want to rush that just for the sake of CI, especially since there's a new feature and a behavior change in on main.

Testing with manual dispatches, ex https://github.com/openforcefield/openff-toolkit/actions/runs/13976249748

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7d753eba6c62`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.