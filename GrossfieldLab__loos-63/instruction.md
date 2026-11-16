# Issue

**Title:** Fixed command line args issue with all_contacts.py

## Description

Resolved a Numpy DeprecationWarning and updated references to modelSelectionOptions and trajOptions in all_contacts.py to fix bug discovered today that prevented all_contacts.py from running

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `142f73ad4f8c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.