# Issue

**Title:** Fix issue #288

## Description

This PR  by checking both the element _and_ ambertype properties when detecting dummy atoms. This avoids a round-trip self-consistency issue when creating `somd1` input from perturbable molecules created via `sire.morph.create_from_pertfile`, where the element property will have been replaced with the value of the end state with the largest number of protons, i.e. dummies may no longer have `Element("Xx")`.

This PR supersedes #287.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6d431c5b8697`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.