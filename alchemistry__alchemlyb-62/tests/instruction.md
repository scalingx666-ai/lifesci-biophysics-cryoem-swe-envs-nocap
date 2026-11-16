# Issue

**Title:** Add pV and U to reduced potential when they are given

## Description

As explained in  The current parser assumes U and pV to be given in the second respectively last column - which is not very robust.

With these changes the parser adds them to the potential differences only when they are given. 

Moreover I am interested if these changes pass the tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eba1f9a651ae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.