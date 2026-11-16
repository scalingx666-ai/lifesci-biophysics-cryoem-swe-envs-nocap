# Issue

**Title:** Fix issue #218

## Description

This PR  by making sure that the _existing_ trajectory backend is used to get the number of trajectory frames from a running process. This fixes the issue reported at OpenBioSim/biosimspace_tutorials#17 and has been tested using the notebook referenced there.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fe41a767379c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.