# Issue

**Title:** Fix the selection of the activation function in CFConv

## Description

`NNPOps.CFConv` used the shifted soft plus activation function regardless the value of the `activation` argument. 

- [x] Fix the selection of the activation function

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db8911d1e4ce`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.