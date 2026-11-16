# Issue

**Title:** Add method to remove the space property from a system

## Description

This PR adds a method to `BioSimSpace._SireWrappers.System` to remove the space property from a system. At present this doesn't warn the user if there is more than one molecule, e.g. it's not a vacuum system. I'm happy to add this if useful. (I imagine the use case here is removing water from a box, then removing the space.)

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc6602e4488f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.