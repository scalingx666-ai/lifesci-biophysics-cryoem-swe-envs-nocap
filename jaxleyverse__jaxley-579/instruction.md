# Issue

**Title:** Make sphinx configuration file explicit

## Description

The most recent readthedocs documentation (v0.6.0) did not build because we now require an explicit reference to the sphinx config file. See [here](https://about.readthedocs.com/blog/2024/12/deprecate-config-files-without-sphinx-or-mkdocs-config/) for more info.

With this change, v0.6.1 should build successfully.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ffafadde7476`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.