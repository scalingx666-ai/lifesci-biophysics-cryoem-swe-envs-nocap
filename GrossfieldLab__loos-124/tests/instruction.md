# Issue

**Title:** Lbf fixup

## Description

---
name: Pull request
about: Create a report to incorporate code
title: 'long-bond-finder fixup'
labels: 'C++ Tools'
assignees: 'Alan'
---

Fixes # .

## Changes proposed in this pull request
    - Changed the way connectivity is checked for (just within a selection now). This fixes an annoying surprise when dealing with .gro files (because they lack connectivity and you don't want to have to infer it across an entire model using only one cutoff). 
    - Made the tool print the frame and offending bond it finds if `--quiet` isn't thrown and `--time-series` isn't provided. This will be the default behavior of the tool if no options are specified now. The return values of the tool will not change.


## Checklist
    - Does this pull request address an open issue? If so, which one? No, I noticed the problem and fixed it in my own usage.
    - Have you applied a formatter to your code (e.g. flake8 or black for python, clang-format for C++)? No, I had applied a formatter previously and believe that my code is still formatted correctly because changes were small.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `88e8adf28a96`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.