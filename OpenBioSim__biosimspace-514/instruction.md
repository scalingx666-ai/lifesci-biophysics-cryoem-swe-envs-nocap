# Issue

**Title:** Fix ring break merge

## Description

This PR updates the `mergeIntrascale` function call and adds a regression test to confirm that the connectivity of merged ring-break intrascale matrices is preserved.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `38ed3b350e87`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.