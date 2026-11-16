# Issue

**Title:** Shallow clone of OpenMM

## Description

* Also disables building the PME plugin for OpenMM which we don't use. 
* Makes building the docker container slightly faster

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3cd2a9762297`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.