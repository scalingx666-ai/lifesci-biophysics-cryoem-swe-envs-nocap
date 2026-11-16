# Issue

**Title:** Fix issue #299

## Description

This PR  but not using `sire.legacy.Base.wrap` for string values in the `property_map`, which were incorrectly being cast to `GeneralUnit`. This allows Mol2 files to be written from by `BioSimSpace.IO.saveMolecules`. This superseds #298.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `598a67435a5f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.