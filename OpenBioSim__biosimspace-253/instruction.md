# Issue

**Title:** Avoid setting all end-state properties when decoupling

## Description

This closes  BSS.Align.decouple() no longer sets LJ1, element1, or ambertype1, rather this is done by the _to_pert_file method of Process.Somd. The related tests of "decouple" have been removed, and direct tests on the SOMD pert files have been added.

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): y
* I confirm that I have permission to release this code under the GPL3 license: y

## Suggested reviewers:
@lohedges, @xiki-tempula

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0334e54801bb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.