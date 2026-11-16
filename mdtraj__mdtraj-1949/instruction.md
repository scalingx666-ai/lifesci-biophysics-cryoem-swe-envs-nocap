# Issue

**Title:** Consolidate/update config files

## Description

A few of these files are outdated, others can be wrapped up into `pyproject.toml`. I personally think TOML is a bad format but I think the benefits of consolidating config into fewer files is a value add.

Some adjacent tasks that would be nice to do in separate PRs are
* Switch version handling to `versioningit` or [`setuptools-scm`](https://github.com/pypa/setuptools-scm)
* Update ... *gestures broadly* the build system
* Look through CI logs for other warnings/noise
* Rename default branch to `main`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2e0f5a4dcc2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.