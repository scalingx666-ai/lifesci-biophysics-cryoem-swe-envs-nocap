# Issue

**Title:** Fix issue #236

## Description

This PR  and closes OpenBioSim/biosimspace_tutorials#23. This is achieved by making the `_generate_binary_run_file` method static, so that it can be called externally, e.g. during the `BioSimSpace.FreeEnergy.Relative` setup process. This is tested via the existing BioSimSpace tests, as well as the tutorial notebook for which the original issue was posted.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dff31ebeca09`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.