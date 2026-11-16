# Issue

**Title:** Fix issue #340

## Description

This PR  by converting the reference system to the chosen end state _before_ writing to a coordinate file to be used for restraints. This ensures that the `coordinates` property is correctly detected.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dc6602e4488f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.