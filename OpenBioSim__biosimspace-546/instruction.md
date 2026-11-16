# Issue

**Title:** Fix issue #545

## Description

This PR  by switching to a native lazy-import system.

Also  and .

Note, this will fail on certain platforms where the Sire CI (spuriously) failed. Will re-run once those complete.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dcff2b0dd96d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.