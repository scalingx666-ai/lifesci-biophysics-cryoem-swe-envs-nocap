# Issue

**Title:** add box attribute to molecule and propagate from pdb parser

## Description

small fix to reading boxes. The PDB reader did not propagate the box attribute

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `54e933d476ef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.