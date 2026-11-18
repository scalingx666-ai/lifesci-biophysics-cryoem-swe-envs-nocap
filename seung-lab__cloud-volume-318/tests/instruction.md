# Issue

**Title:** feat: making sharded mesh readers for graphene, added decoder for labels

## Description

We are adding the capability to decode graphene segids and read "initial" L2 meshes that will be created in the sharded format.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ae6e2194c92a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.