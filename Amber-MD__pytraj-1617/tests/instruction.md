# Issue

**Title:** Fix memory leak on topology mask selection

## Description

Had a little look at the memory leak bug Amber-MD/pytraj#1615 as it was also annoying me.

Needed to delete the newParm pointer created by cpptraj after assignment as it remained on the heap. Not sure if this is the best way to handle it but it fixes the memory leak.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cff5bb3a4145`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.