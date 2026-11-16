# Issue

**Title:** Add script to check files for complete type hints (fix #248)

## Description

In this PR, I try to adress issue #248 and my previous 

The script checks Python files in the project for complete type hint annotations:
1) Scans the 'prolif' and 'tests' directories to find all Python files
2) Runs mypy with the "--disallow-untyped-defs" flag to identify files missing type hints
3) Builds and displays a tree structure showing which files have complete type annotations
4) Marks files with complete type hints as [x] and files missing type hints as [ ]
5) Provides a summary showing what percentage of files have complete typing

I have implememted the use of json to make if more efficient. Before I was running mypy in each file

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5bf7456abb5c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.