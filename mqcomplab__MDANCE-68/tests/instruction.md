# Issue

**Title:** chore: updating scripts and tutorials to reflect the newest NANI changes

## Description

Motivation: because the default hyperparameters in NANI has been changed. We need to update scripts, tutorial, docs to reflect this

Other changes: fix bug in NANI `execute_kmeans_all` turn into else statement.

Effect: `screen_nani.py` has been improved. Note that stratified sampling is similar to old diversity selection, where NANI is screened once for the highest `k` and for every `k-1` is the initiator is just minus last element. The trick still retains meaning that it is very fast to screen. 

Docs has been updated as well with new links, citations, etc.

We are ready to push this for new version

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `00d0361c7217`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.