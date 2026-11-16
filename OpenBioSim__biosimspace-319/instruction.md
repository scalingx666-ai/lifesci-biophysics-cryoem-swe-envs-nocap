# Issue

**Title:** Fix issue #318

## Description

This PR exposes the `extra_args` kwarg for `BioSimSpace.Process.Somd`. This is available for the other process classes but had (for some reason) been omitted for this one. I've tested by running charge change perturbations with SOMD, e.g. by passing `extra_args={"-q": -1"}`.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@mb2055

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `14e376c3df13`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.