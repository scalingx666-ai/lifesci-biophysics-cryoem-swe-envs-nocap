# Issue

**Title:** Fix AMBER FEP mappings

## Description

This PR adds the missing mapping pruning functions from the Exscientia, which are required for stable mappings when using AMBER as the FEP engine. This functionality is tested within the sandpit. I've also tidied a few bits of the mapping code and made sure that sub-functions take kwargs so that we don't get bugs if the order of inputs changes in future.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8f8c2b95b1c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.