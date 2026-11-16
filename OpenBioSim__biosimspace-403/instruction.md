# Issue

**Title:** Fix issue #402

## Description

This PR  by replicating the functionality from the `getSystem()` method to make sure that systems returned by `getFrame()` are unsquashed, thus are compatible with the input system used to create the process.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ed2d1f1dd9d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.