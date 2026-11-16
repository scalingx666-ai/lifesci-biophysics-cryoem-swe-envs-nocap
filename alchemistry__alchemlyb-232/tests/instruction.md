# Issue

**Title:** AMBER parser now compare user-provided T with what's read from AMBER files

## Description

This PR addresses issue #225, now:

- we require ```T``` in input for the parser
- we still read ```temp0``` from AMBER output
- the WARNING about non-constant T simulation is now a WARNING (the check could also be removed)
- the parser raises a WARNING if ```temp0 != T``` (two new tests now assert that)
- now the tests in ```test_amber.py``` use the correct  T=298.0

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `97ac3d95406c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.