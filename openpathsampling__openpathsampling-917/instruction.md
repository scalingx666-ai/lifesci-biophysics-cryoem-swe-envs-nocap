# Issue

**Title:** Set reject_bytes=False for ujson >= 3

## Description

Since `ujson` version `3.0.0` tests based on openpathsampling saving start failing.

`ujson.dump()` now has default keyword `reject_bytes=True` which leads to issues for dumping CV's (which contain bytestrings).

I had to do some import `if` statements as well as this keyword is not valid for `ujson` = `2.x`  (And I supposed you still wanted to support that version.)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `642fcde5e123`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.