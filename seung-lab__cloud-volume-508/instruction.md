# Issue

**Title:** make contain more accurate for boundary points

## Description

following the convention of python indexing, the `maxpt` should not be inclusive?
according to the measurement of `size` or `length`, it seems that this is true.
if so, the point should not be contained if it is on the coordinate of `maxpt`?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15672a3169e9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.