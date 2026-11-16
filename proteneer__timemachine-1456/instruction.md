# Issue

**Title:** Add nn handlers

## Description

- Add NNHandler and EnvironmentNNHandler to be able to use NN features to parameterize BCC like corrections to the underlying AM1/Amber charges respectively
- NN is simple feedforward fully connected network with SiLU as the activation function
- Add/update tests

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a47e20e0c0af`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.