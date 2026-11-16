# Issue

**Title:** fix idr annotation for multiple regions being given

## Description

if multiple regions are given to `id-regions`, then currently they get overwritten so only the last one is actually annotated. This fixes that by checking if the annotation has already been applied

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2cc128ee29a0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.