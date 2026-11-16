# Issue

**Title:** Improve error mesage when attempting to extract all dummy selection.

## Description

This PR  by catching a failing search for non dummy atoms in the molecule prior to extraction. Previously this was converted to an empty list, which failed at the point we attempted to extract the empty selection from the molecule.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0334e54801bb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.