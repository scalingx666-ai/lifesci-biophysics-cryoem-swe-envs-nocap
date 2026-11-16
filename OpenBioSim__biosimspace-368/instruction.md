# Issue

**Title:** Improve error messages if no lambda windows found during analysis

## Description

# Is this pull request to fix a bug, or to introduce new functionality?

If users have generated output without using BSS, the directory structure may not be as BSS expects when parsing lambda windows from directory names. Previously, this lead to a an empty `lambdas` list, and `BSS.FreeEnergy.Analyse` returned `([], correct_overlap_matrix)` without raising an error. The code now explicitly raises an error.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): y
* I confirm that I have added a test for any new functionality in this pull request: n
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: n
* I confirm that I have permission to release this code under the GPL3 license: y

## Suggested reviewers:
@lohedges

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b145e30fbea4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.