# Issue

**Title:** Compile torch extension in setup.py

## Description

Currently the neighbor extension requires having nvcc installed when importing it.

This PR makes it so that the extension is compiled when torchmdnet is pip installed. This removes nvcc as a runtime dependency, which solves the problem of packaging it in the upcoming conda-forge package #215

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ac16c09069f3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.