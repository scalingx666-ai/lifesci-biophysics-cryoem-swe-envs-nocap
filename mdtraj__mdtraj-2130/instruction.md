# Issue

**Title:** move most items from setup.py to pyproject.toml

## Description

A total cleanup of `setup.py`, moving mostly everything (bar the extension building portions) into `pyproject.toml`, following the general direction of what PyPA wants. 

The only major thing that needs to be changed is that we still list Rob McGibbon as the sole author.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f1b285a7bcd0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.