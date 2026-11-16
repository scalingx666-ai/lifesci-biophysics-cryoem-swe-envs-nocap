# Issue

**Title:** add arg to MemmappedDataset

## Description

The AceDataloader (ace.py) is referring to MemmappDataset as super class and it's complaining about `remove_ref_energy` arg when it's initialized. This small change, it's fixing it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fdd4dac8852f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.