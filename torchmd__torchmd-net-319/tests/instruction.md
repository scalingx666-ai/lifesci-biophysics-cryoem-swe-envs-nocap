# Issue

**Title:** Enable allow_pickle to be able to specify splits.

## Description

I've been trying to train a torchmd-net model following the examples in the examples folder but I'm unable to specify my own splits. This is an easy fix that unblocks me and perhaps other users. If you know of a way to save the splits in a way that doesn't require us to enable allow_pickle I'd love to hear about it and we can close the PR. 

Full disclosure about the level of testing - this change allows me to get past the part of the code where the splits are loaded but I'm currently hitting other errors later, so I can't yet verify that the full script runs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `552e0eeb16ce`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.