# Issue

**Title:** More robust parsing of quaternions from DUMP files.

## Description

Parsing of the DUMP file changed for quaternions. Now, MOLTEMPLATE expects the quaternions to be given in the order XYZW if they are named c_orient, or WXYZ if they are named c_q. Help entry updated accordingly.

I have done so because Ovito expects the c_orient compute to have the order i,j,k,w, while so far we have used the convention w,i,j,k.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `57fc6fcf6bc6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.