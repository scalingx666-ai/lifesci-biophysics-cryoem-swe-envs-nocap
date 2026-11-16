# Issue

**Title:** Correct behaviour and improves `selaltloc`

## Description

In some cases, the residue previous to the `altloc` group was discarded. This PR corrects that behaviour.

* the solution adds a **single** line of code to the algorithm
* all previous tests pass
* add more tests
* add more documentation to tests
* removed one tests that was not correct
* add a file that was missing in #131

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b0b7bd5c59fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.