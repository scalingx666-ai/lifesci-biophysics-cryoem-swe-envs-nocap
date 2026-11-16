# Issue

**Title:** Parser Utils extra exception info

## Description

Also 

This current PR so far was made with the intention to avoid breaking changes, it so far adds whatever info is readily available to parse error messages (I figured this already partly fixes the issue without the breaking change of adding paths to function signatures).

I also changed two regexes to raw strings, since there were warnings printed while using the martinize2 script (I hope this is ok)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c7f8f4abe46`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.