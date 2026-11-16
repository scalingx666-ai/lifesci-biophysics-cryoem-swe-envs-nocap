# Issue

**Title:** Fix issue #332

## Description

This PR  by relaxing the constraint on the alignment of single atom mappings, i.e. they no longer need to involve an ion. The original bug was caused by the mapping having too few degrees of freedom, not the molecule itself.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c837b814340f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.