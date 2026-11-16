# Issue

**Title:** Update makeCompatibleWith to use the new AtomCoordMatcher

## Description

This PR updates the `makeCompatibleWith` function to use the new `AtomCoordMatcher`, which should hopefully fix issues where `tLEaP` moves a atom between residues. I'd like some time to test this before the next release in case there are some unforeseen consequences of this switch, or edge cases that aren't covered by the existing tests. It might be necessary to use the old match as a starting point, then fall back on this one if it fails.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

@msuruzhon: Tagging you in since this is relevant to your work.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15e37d9683bd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.