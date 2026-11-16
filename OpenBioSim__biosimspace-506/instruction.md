# Issue

**Title:** Fix GLYCAM force field merging

## Description

This PR builds on OpenBioSim/sire/pull/408 by updating the intrascale merge to use the new C++ `mergeIntrascale` routine. The PR also adds some optimisations to the ring breaking / growing connectivity checks that caused performance issues for large protein merges.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `99cef0dbc034`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.