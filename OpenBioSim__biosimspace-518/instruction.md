# Issue

**Title:** Add ring-breaking/making info during merge and add support for adding ions to an existing system

## Description

This PR flags atoms involved in ring-making/breaking bonds during the merge process as a molecule property, allowing them to be processed by downstream tools, e.g. adding soft-core forces in the Sire-OpenMM conversion layer.

The PR also adds support for adding ions to an existing system, i.e. not just during solvation. This also standardises and improves the ion concentration calculation used internally within the solvate function.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [y/n]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d7d3fc8020b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.