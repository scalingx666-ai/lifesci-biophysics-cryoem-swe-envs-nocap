# Issue

**Title:** Fix PBC checks, make pbc and box length info accessible to RandomWalkState

## Description

### PR Summary:
This fixes an issue with checking for overlaps along PBCs, adds a test that should have caught it. This also makes pbc information and box lengths accessible to RandomWalkState

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a1cac35ab678`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.