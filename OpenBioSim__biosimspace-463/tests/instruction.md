# Issue

**Title:** Fix issue #462

## Description

This PR  by fixing the name of the context manager. I've also updated the docstring to state that inputs need to use absolute paths when a working directory is set, i.e. since paths will no longer be relative to the working directory of the Python process.

Also  by fixing a typo in an RDKit d2d draw option. This worked previously, so either there was a typo in the RDKit API that has been fixed, or the RDKit code no longer allows invalid attributes to be set.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1096b37d03a6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.