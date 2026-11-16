# Issue

**Title:** Fix issue #354

## Description

This PR  by rotating perturbable and non-perturbable molecules _independently_ when rotating a simulation box as part of a reduction. Previously both sets of molecules were rotated at the _same_ time, using the property map to specify the name of the coordinates (or velocity) property to use. This would raise an exception for the first molecule without this property, hence the actual perturbable ligand was never rotated.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

Tagging @mark-mackey-cresset so that you can test this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37ec978f8e47`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.