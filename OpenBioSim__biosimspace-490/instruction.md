# Issue

**Title:** Just set intrascale on the basis of the connectivity

## Description

This PR simplifies the calculation of the end-state intrascale matrices, basing it entirely on the end-state connectivity determined from the bonding. The complicated logic is no longer needed, which (originally) assumed that your were building the matrices from the end-state intrascale properties, which is no longer the case.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aa0c8f7fae7b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.