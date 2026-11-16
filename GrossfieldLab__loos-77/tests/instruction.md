# Issue

**Title:** Long bond finder and python uplift

## Description

---
name: Pull request
about: Create a report to incorporate code
title: 'Look for periodicity issues by finding overlong bonds'
labels: 'cli-tools, traj-processing, imaging-issue-check'
assignees: 'agrossfield'
---

Fixes no issue

## Changes proposed in this pull request
    - wrote tool to efficiently search for overlong bonds in trajectories
    - added key methods to atomic group for use in pythonic contexts
    - changed python typelifting of AG iterators to be concise and efficient (call `list()` instead of for-loop-append idiom).


## Checklist
    - Does this pull request address an open issue? If so, which one?
        - No
    - Have you applied a formatter to your code (e.g. flake8 or black for python, clang-format for C++)?
        - Yes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d841d73a63f7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.