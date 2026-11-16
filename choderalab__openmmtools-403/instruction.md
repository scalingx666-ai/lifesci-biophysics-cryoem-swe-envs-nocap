# Issue

**Title:** Minor travis cleanup

## Description

This PR updates `.travis.yml` to:
* Add a python 3.7 test branch that tests against `omnia-dev` development OpenMM nightly builds
* Cleans up some cruft that was supposed to have been culled out when OpenMM 7.3 was released and Python 2.7 was dropped

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d472e82bbd96`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.