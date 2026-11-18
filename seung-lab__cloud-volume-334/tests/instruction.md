# Issue

**Title:** perf: instant fs based shared memory files + use sparse file capability

## Description

Previously we were writing out real files full of zeros,
but a) that takes forever and b) modern OSes support sparse
files (i.e. gigabytes of zeros that take up only a few real bytes).

The following should take advantage of this functionality and be faster.
It should work on Python 2.7 Unix, and Python 3.5+ on Unix and Windows.

References:
  https://stackoverflow.com/questions/8816059/create-file-of-particular-size-in-python
  https://docs.python.org/3/library/os.html#os.ftruncate
  https://docs.python.org/2/library/os.html#os.ftruncate

Hopefully the use of spare files will help prevent issues like one user experienced where an error ate up his hard drive's free space on Windows.

This issue might fix #222

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ffb2937ef159`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.