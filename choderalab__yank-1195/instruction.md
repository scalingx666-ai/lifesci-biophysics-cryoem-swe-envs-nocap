# Issue

**Title:** Fix bug causing crash when resuming redistributed protocol

## Description

I found another small bug that causes YANK to crash when trailblaze is interrupted and then resumed with the option `bidirectional_redistribution` set.

I'm going to make another bugfix release after merging this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f940cfacff59`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.