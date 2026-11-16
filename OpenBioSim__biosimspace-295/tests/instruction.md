# Issue

**Title:** Don't perturb LJ sigma for ghost atoms

## Description

This PR switches to not perturbing the LJ sigma parameter for ghost atoms so that merged molecules created by BioSimSpace are compatible with  SOMD2 by default. We still preserve the existing behaviour (zero sigma and epsilon) for SOMD1. The PR also includes a few modifications to harden the checking of dummy atoms during the pert file writing stage. (This isn't needed during merging, since we are explicitly setting the appropriate molecule and atom properties.)

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `598a67435a5f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.