# Issue

**Title:** binary build enhancement

## Description

- Only support python 2.7, 3.5, 3.6, 3.7
- Use `condaforge/linux-anvil` docker image for conda build but using `pytraj-build-box:2019-03` (centos-5 based) for pip build.
-

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `61cb9d23e294`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.