# Issue

**Title:** Update CI.yaml

## Description

Now that we merged in the openmoltools pr 

and parmed merged in my pr to fix the namespace issue


We can install from master instead of my forks. Once a new release gets cut we can just install from conda.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ac5cd56e1383`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.