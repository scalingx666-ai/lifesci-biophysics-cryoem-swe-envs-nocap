# Issue

**Title:** Fix issue #523

## Description

This is an attempt to fix issue #523. I think this should resolve issues on certain platforms/variants, but it also looks like `sqm` from `ambertools` is borked, likely due to (yet another) `glibc` incompatibility issue.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `329cf7634f03`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.