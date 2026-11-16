# Issue

**Title:** Fix forgotten change to setup.py for inputs subpackage

## Description

@tczorro This may also be related to the version problem you were facing. I'll merge this soon.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a7000f83e837`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.