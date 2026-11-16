# Issue

**Title:** Edit on the polymer.py to handle hydrogen residue during End group Addition

## Description

### PR Summary:

An issue was raised on extra hydrogen bond remains during chain formation with end_groups in polymer. Added the hydrogen removal logic (the primary contributed might have missed by mistake) which fixed the invalid bond formation issue, and allowed the proper force field generation using foyer without error.

The issue is discussed properly in  #1320.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53013f1e784f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.