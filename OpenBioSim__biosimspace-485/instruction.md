# Issue

**Title:** Fix issue #484

## Description

This PR  by updating the default GROMACS NPT configuration to match the Recursion sandpit. The updated defaults satisfy the constraint on `tau-p`, which is now strictly enforced by `gmx grompp`.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1d064c0efdd9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.