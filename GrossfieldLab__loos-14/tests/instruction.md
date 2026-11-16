# Issue

**Title:** Fullhelper

## Description

fullhelper.py is a small python script that takes a text file containing  a full-help string, a C++ source file, and a string to be substituted for (the signal), then inserts the full-help string with formatting so that each line has correct quotation and newlines added to it for writing to cout. as an optional last argument it can take the number of columns you'd like to wrap the full-help string to. 

It prints the string after formatting it so you can quickly iterate if you need a different wrapping. It also creates a backup of the source before subbing the full-help string, in case you'd like to revert to before you'd made the substitution. It also comes with a full-help and usage message.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9a1679c15902`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.