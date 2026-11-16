# Issue

**Title:** Additional LOOSError definition (Fix for Issue #110)

## Description

Attempted fix for Issue #110
Introduced new polymorphic LOOSError definition that includes a filename argument and writes out the filename in the error.

Also used the new function in the LOOSError throws in the AmberNetcdf trajectory reader amber_netcdf.cpp

---
name: Pull request
about: Create a report to incorporate code
title: ''
labels: ''
assignees: ''
---

Fixes # .

## Changes proposed in this pull request
    -
    -
    -


## Checklist
    - Does this pull request address an open issue? If so, which one?
    - Have you applied a formatter to your code (e.g. flake8 or black for python, clang-format for C++)?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5e5c5d26e276`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.