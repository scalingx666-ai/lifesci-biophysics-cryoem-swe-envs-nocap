# Issue

**Title:** Make zeroing of dummy atom LJ sigmas optional

## Description

This PR adds a new kwarg to the `merge` function to allow the user to choose whether to zero the LJ sigma value for dummy atoms.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d618222c4c5a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.