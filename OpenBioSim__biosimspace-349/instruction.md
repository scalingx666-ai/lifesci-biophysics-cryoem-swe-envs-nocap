# Issue

**Title:** Fix issue #348

## Description

This PR  by using direct conversion to/from and RDKit format intermediate for parameterisation with OpenFF. This avoids issues using intermediate file formats, e.g. PDB or SDF. We first convert from SIre/BioSimSpace to RDKit, then create an OpenFF molecule directly from this using the `Molecule.from_rdkit()` functionality.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `71927d10d952`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.