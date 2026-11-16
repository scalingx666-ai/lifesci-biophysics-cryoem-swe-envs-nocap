# Issue

**Title:** Allow to configure the depth of the MLP in output modules

## Description

Adds an MLP module to utils and uses it in the output models.

Allows to configure the depth of it via a train.py option, `output-mlp-num-layers`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `74702dad9431`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.