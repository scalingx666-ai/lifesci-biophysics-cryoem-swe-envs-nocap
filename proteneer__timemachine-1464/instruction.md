# Issue

**Title:** Fix Invalid Memory Write in TIBD Exchange Mover

## Description

* Resolves a memory issue where a kernel could write beyond the length of a buffer. Failing test in 
* Adds padding between memory allocations for memory checking in `compute-sanitizer`. Doesn't catch any new issues, but improves chances to catch issues.
* Minor clean up of some code

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `30d480633f18`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.