# Issue

**Title:** Fix issue #347

## Description

This PR  by allowing the Antechamber `acdoctor` mode to be disabled via a kwarg when using `BioSimSpace.Parameters.gaff/gaff2`, or via `BioSimSpace.Parameters.parameterise`.

I'll re-trigger the CI once the Sire development packages have finished building.

@noahharrison64: Tagging you in so that you can test.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb44a6fde5c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.