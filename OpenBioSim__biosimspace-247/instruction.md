# Issue

**Title:** Fix issues #240 and #246

## Description

This PR...

 by using the default Sire extensions for the interchange prmtop and inpcrd files. In particular, this ensures that the coordinate file isn't accidentally detected as an SDF file.

 by fixing the range of the funnel axis used in the calculation of the correction term.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d94c19baceeb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.