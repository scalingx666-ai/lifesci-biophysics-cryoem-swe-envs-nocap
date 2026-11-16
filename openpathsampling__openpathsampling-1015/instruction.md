# Issue

**Title:** rename random => rng

## Description

This renames the `openpathsampling.random` module () to `openpathsampling.rng`. The fact that the name `random` shadows the std library's `random` when the main OPS source directory is my cwd was wreaking havoc with scripts I use in my usual workflow.

I'm open to other possible names for the module -- just that `random` is causing me too many headaches.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e73165002c83`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.