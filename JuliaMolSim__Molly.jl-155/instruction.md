# Issue

**Title:** Fix SimpleCrystals Bugs

## Description

Tests pass locally. Forgot that Molly stores coords as `Vector{SVector{N,T}}` so just had to update that in the Crystal constructor to fix things as SimpleCrystals no longer stores its positions as `Vector{SVector{N,T}}`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0c58f26be866`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.