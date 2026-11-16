# Issue

**Title:** Allow user to force stereo inference

## Description

This PR is a companion to OpenBioSim/sire#290, which exposes the `force_stereo_inference` kwarg on `BioSimSpace.Convert.toRDKit()`. This has been tested via Sire itself.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c64953636b05`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.