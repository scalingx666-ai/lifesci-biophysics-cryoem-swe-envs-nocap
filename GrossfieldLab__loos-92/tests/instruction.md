# Issue

**Title:** make netcdf support mandatory, fix #91

## Description

---
name: Make netcdf support mandatory
about: 
title: 'make netcdf support mandatory, fix #91'
labels: ''
assignees: ''
---

  .

## Changes proposed in this pull request
    - Removed all instances of the HAS_NETCDF conditional compilation, since we always want to support it 


## Checklist
    - Does this pull request address an open issue? #91 
    - Have you applied a formatter to your code (e.g. flake8 or black for python, clang-format for C++)?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `50afdb80adbc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.