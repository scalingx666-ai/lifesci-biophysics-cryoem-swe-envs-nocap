# Issue

**Title:** Extend saved atomic charges for fchk

## Description

Add new code to `fchk.py` to be able to extend the default saved set of charges to:
- MBS charges ( minimum basis set Mulliken population)
- Type 6 charges (Hirshfeld)
- Type 7 charges (cm5)

I checked the log file to assigned Type 6 and 7 but I couldn't find gaussian documentation to confirm it. I added a new test in test_fchk.py and a new fchk file to data to be able to test the new code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `964a21769b63`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.