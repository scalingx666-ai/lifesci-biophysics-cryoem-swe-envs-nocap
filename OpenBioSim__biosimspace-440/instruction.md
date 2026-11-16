# Issue

**Title:** Add support for renumbering molecules when copying a system

## Description

This PR  by adding a `renumber` kwarg to the `System.copy()` method. When `True`, molecules in the system are given a unique number. The default behaviour is `renumber=False`, for consistency with the existing code and for performance.



* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8d7eb9c3d6f5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.