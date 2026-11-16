# Issue

**Title:** Clashscore2

## Description

Adds a clashcore2 program that mirrors the clashscore program but it uses mmtbx.reduce2 and mmtbx.probe2 rather than phenix.reduce and phenix.probe.

It takes the same command-line programs as the original except for time_limit and it also supports probe PHIL parameters.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `65e4c01dbccd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.