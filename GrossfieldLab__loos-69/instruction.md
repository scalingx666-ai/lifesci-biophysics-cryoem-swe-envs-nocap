# Issue

**Title:** correctly export AtomicGroup.splitByName to python

## Description

---
name: Pull request
about: Create a report to incorporate code
title: 'Set swig to correctly export AtomicGroup.splitByName'
labels: ''
assignees: ''
---

 

## Changes proposed in this pull request
    - Change loos.i and AtomicGroup.i
    - Set the return of AtomicGroup.splitByName() to be a python dictionary



## Checklist
    - Does this pull request address an open issue? If so, which one?
    - Have you applied a formatter to your code (e.g. flake8 or black for python, clang-format for C++)?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f64766570a9c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.