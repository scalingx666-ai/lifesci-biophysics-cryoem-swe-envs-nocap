# Issue

**Title:** bugfix: addtoconfig is not a function in _amber.py, add to protocol d…

## Description

…ict instead

Hello! This is a PR to fix a small bug in the amber config file writing. As there is no addToConfig() function, this throws an error. Adding to the protocol dict instead fixes this line.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): y
* I confirm that I have permission to release this code under the GPL3 license: y

## Suggested reviewers:
@lohedges

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fe41a767379c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.