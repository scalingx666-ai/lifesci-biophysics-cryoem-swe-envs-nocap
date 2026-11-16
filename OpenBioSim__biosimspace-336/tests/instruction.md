# Issue

**Title:** Fix issue #335

## Description

This PR  by ensuring that the underlying Sire object of a BioSimSpace system is updated during the inner loop when updating molecules within the system. Otherwise, only the final molecule will be updated.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f692128267f0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.