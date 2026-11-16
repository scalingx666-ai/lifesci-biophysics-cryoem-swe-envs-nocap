# Issue

**Title:** Add support for focused sampling with SOMD2

## Description

This PR modifies the SOMD2 parquet extraction code to always use the lambda values in the table, minus those used for gradients, rather than the `lambda_array` metadata, which has now been deprecated. This allows sampling of different lambda windows over time for restart simulations, e.g. if you want to focus much more sampling on a particular region of lambda space.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `baad60690aff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.