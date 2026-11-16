# Issue

**Title:** Feature mapping diagnostic

## Description

`BioSimSpace.Align.matchAtoms` now tells users when it has probably returned a sub-optimal mapping. After computing the default MCS it looks for mapped atoms that have an unmapped heavy-atom neighbour of the same element on both molecules, i.e. attachment points where the search stopped symmetrically, which usually means a pairable atom was missed. If it finds any, it re-runs the MCS with `ringMatchesRingOnly` relaxed and compares: only if the second mapping preserves every original pair and extends it sensibly (heavy to heavy, hydrogen to hydrogen) does it emit a `UserWarning` suggesting `mcs_kwargs={'ringMatchesRingOnly': False}`. The check is purely observational, i.e. the originally computed mapping is always what gets returned, and it fires only where the advice can be acted on, so it is suppressed for internal callers such as `rmsdAlign`, `flexAlign`, the ROI path and ATM setup, which offer no way to pass MCS options through. The existing test set produces no false positives, but this is something that should be monitored over the current development cycle.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have added a test for any new functionality in this pull request: [y]
* I confirm that I have added documentation (e.g. a new tutorial page or detailed guide) for any new functionality in this pull request: [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `73342c7a3d34`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.