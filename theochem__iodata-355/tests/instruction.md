# Issue

**Title:** Require optional arguments of iodata.api functions to be specified with keywords

## Description

This fixes one of the tasks in #191, deferred from #352.

This way, scripts using IOData become more readable, lower risk of bugs. It also gives us the freedom to reorganize optional arguments with less risk of breaking API compatibility. This change is in itself API-breaking.

I will YOLO-merge this on Friday, June 5, unless reviewed earlier.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8cf113273916`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.