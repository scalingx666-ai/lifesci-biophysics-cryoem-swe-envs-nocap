# Issue

**Title:** Update `.readthedocs.yaml`

## Description

I think this fixes the failing docs, as shown on the README badge:
[![Documentation Status](https://readthedocs.org/projects/nequip/badge/?version=latest)](https://nequip.readthedocs.io/en/latest/?badge=latest)
From: https://github.com/mir-group/nequip

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bffd533c60a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.