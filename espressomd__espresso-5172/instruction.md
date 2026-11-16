# Issue

**Title:** Modernize zndraw interface

## Description

, 

- Removed unused/unnecessary code
- Fixed zndraw interface not respecting PBC
- (Re-?) Introduced bonds to zndraw
- Ready for current zndraw version (0.5.11)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `97f1853a48f8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.