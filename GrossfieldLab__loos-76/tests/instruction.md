# Issue

**Title:** Fix LoosOptions pre-made command line options

## Description

---
name: Fix problems with the new LoosOptions module
about: Create a report to incorporate code
title: ''
labels: ''
assignees: ''
---

 and (partly) #72 

## Changes proposed in this pull request
    - Changes the way fullhelps are handled in LoosOptions (#71)
    - added descriptions to the tools that use LoosOptions (#71)
    - started getting rid of required options, since that breaks help and fullhelp (partially address #72)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e06eea710b70`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.