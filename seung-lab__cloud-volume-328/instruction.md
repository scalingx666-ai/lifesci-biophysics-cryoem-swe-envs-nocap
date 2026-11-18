# Issue

**Title:** refactor: drop pylru for custom version

## Description

Noticed pylru is GPL and am trying to keep CV BSD. This cures the license issue.

I looked around and all the other implementations either are not as convenient for some reason or don't have the right license, so I wrote my own. 

As a bonus, the new `LRU` class includes a `resize(N)` method, so it's easy to customize after the fact without losing your cache.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f7fda866240c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.