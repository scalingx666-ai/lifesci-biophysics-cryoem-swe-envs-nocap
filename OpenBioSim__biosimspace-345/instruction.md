# Issue

**Title:** Fix issue #344

## Description

This PR  by reverting the way in which the connectivity property is set. While we generally handle perturbations that _don't_ change connectivity, we do require that the end state properties differ, These properties will be used by Sire to infer situations where the perturbation involves a connectivity change.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `71927d10d952`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.