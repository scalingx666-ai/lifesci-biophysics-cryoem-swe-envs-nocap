# Issue

**Title:** Setup miniconda v2 & use new CF OpenMM

## Description

Adressing two things:

* `add-path` and `set-env` commands are deprecated and will be disabled on November 16th. More information see: https://github.blog/changelog/2020-10-01-github-actions-deprecating-set-env-and-add-path-commands/
* I merged the new conda forge packages for OpenMM so they use the `test` label (and not `testing`) following what CF is doing for `cudatoolkit`. This will allow us to test the latest builds!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7c5b932c3b8b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.