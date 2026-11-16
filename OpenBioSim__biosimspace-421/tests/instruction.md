# Issue

**Title:** Add support for the ABCG2 charge method

## Description

This PR adds support for the new ABCG2 charge method from `ambertools` 2024. Currently this can't be tested since the build pulls in `ambertools` 2023 due to dependency constraints. However, I've tested the logic locally and it correctly detects when ABCG2 is available.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15e1a7d7e13c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.