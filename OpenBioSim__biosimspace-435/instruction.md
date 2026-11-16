# Issue

**Title:** Fix issue #434

## Description

This PR  by using `array.tolist()` rather than `list(array)`. The PR also adds custom `__str__` methods to each ATM protocol, since the base class implementation is not appropriate for all protocols, e.g. it assumes that certain methods exist, which might not be the case.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d280272cbd96`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.