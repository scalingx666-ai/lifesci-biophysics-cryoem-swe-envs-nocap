# Issue

**Title:** ASA calculation performance tweaks

## Description

The runtime gain should be around 1.2-1.3x. And together with #1147 it is about 1.3-1.5x 

Implemented by Claude and fully tested. Output is identical to the previous implementation.

The changes are to replace objects with flat primitive arrays so the hot loops are sequential scans over contiguous memory instead of pointer chases.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a57e199c763`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.