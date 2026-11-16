# Issue

**Title:** add small molecule charge change support with test

## Description

## Description

add small-molecule charge change support and attempt to generalize the existing protein mutation charge change functionality to avoid code duplication

## Motivation and context

adds 

## How has this been tested?

performs a `RelativeFEPSetup` on two `Bace` ligands with unequal charge, performs energy validation and asserts both systems are neutral after modification

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4901ccb68b47`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.