# Issue

**Title:** Volume shooting point selector

## Description

This adds a volume-based shooting point selector. It will only select points from a given volume.

(I thought we already had this? I thought this was implemented as part of "Shooting Range" by @hejung in one of our ESDWs? Guess that code never made it into main. Not too hard to recreate this much of it, though.)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d833c5cf4bde`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.