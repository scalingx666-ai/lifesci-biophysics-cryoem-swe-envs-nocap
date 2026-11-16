# Issue

**Title:** Multi-class particle picking: GE-multinomial implementation

## Description

Implementation of GE-multinomial using Gaussian approximation of multinomial prior (in methods.py.) Also made small changes to linear classifier output heads and training commands to accept multi-class input (in training.py and train.py).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1ca87c456bf1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.