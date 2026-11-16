# Issue

**Title:** Handle normalised lambda and lambda_grad metadata entries

## Description

This PR is a fix to handle the updated parquet metadata in `somd2`, which is required to match the updated energy trajectory record formatting in Sire.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f6c9979a0fd6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.