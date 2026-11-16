# Issue

**Title:** Cleanup XTC code and replace Sun XDR code

## Description

by removing the Sun XDR code.  It turns out that there is a large amount of code within and elsewhere that was completely unused by the XTC reader/writer, and the remaining 3 functions were simple enough to implement ourselves.

The tests only wrote and then read files, which wouldn't catch if the reader and writer were both incorrect in the same way.  I added a new test to read a file written by MDTraj, write it, and make sure the result matches the reference file.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ef4294d0545e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.