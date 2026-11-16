# Issue

**Title:** Fix failing windows CI

## Description

Possible fix for #567

## Description
Pin the python package version in an attempt to fix the failing windows CI; see issue #567 

## Todos
- [x] Implement feature / fix bug

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d16b9cc8b8ef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.