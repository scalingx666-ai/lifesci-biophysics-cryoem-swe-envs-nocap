# Issue

**Title:** Fix issue #424

## Description

This PR  by shifting the lower and upper bounds used for data truncation by the start time for each data frame. In addition, the bounds are only determined using fractions of the time interval between the first and last samples, which allows for the sample spacing to change over time. This is all the `alchemlyb` code uses internally, i.e. it just drops data outside of the lower and upper bound.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7b3837a5e8de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.