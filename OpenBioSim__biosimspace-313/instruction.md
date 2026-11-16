# Issue

**Title:** Fix issue #312

## Description

This PR  by skipping RMSD alignment when merging monatomic ions with other molecules, e.g. water when creating an alchemical ion. When a single item mapping is detected and one of the molecules has a single atom, then we skip the alignment and simply replace the coordinates of the mapped atom with those of the reference. A similar approach could be applied in the mapping/merging code with Sire.

(Note that there is some code duplication below since we still have two scoring functions, one for RDKit mappings, and one for Sire.)

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ede60ba7dbd0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.