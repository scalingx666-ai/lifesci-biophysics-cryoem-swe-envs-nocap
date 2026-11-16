# Issue

**Title:** pdb.py renamed

## Description

Having a pdb.py creates conflicts with python's pbd, Python DeBugger.
pdb.py is renamed to writepdb.py to avoid this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `51ba8b8213d3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.