# Issue

**Title:** Fix issue #330

## Description

This PR . It is another attempt at the same logic, which (for some reason) doesn't cause the previous CI failure seen in the other PR. It also includes a small bug fix for the reference system modifications in the AMBER process. I had forgotten to create a local copy of the private object, so there was an attribute error when accessed.

Also  by replacing the example mapping visualisation with the new plot.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0cf9f5fa1450`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.