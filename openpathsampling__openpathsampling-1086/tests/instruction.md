# Issue

**Title:** SimStore: Fix issue with internalized snapshot box vectors

## Description

This fixes a bug when using `engines.features.box_vectors` in SimStore. Previously, these were not saved.

As far as I can tell, this bug only occurs if you are using the internalized Gromacs snapshots, introduced in #933, and storing them with the new SimStore storage system. (However, it could be an issue for engines that aren't included in core OPS.) The OpenMM engine uses a box vectors feature in `engines.features.statics` and the Gromacs engine uses a box vectors feature in `engines.external_snapshots.features.box_vectors`, so neither of these are affected. I only found this bug when I was going to suggest using SimStore + internalized snapshots as part of a [user question](https://mattermodeling.stackexchange.com/questions/6967/) that came up today. It seems that I overlooked this feature when adding SimStore storage support.

New tests fail without the (simple) fix in the feature file and pass with the fix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7443c83d8732`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.