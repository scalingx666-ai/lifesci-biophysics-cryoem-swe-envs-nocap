# Issue

**Title:** Fix bug in extending beads when including a compound

## Description

### PR Summary:

`points.py` extends existing_coordinates when using include compound, but it needs to also extend the bead names. Not doing this was causing issues when including a Bias in the initial walks. 

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `befe731891f5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.