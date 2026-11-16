# Issue

**Title:** Fix pdb_selres on piped input files

## Description

Python's `sys.stdin` is not seekable apparently, so we cannot rewind the contents of the data after reading once. Running the script now one something like `pdb_fetch 1ctf | pdb_selres -80:` will not return anything because the file is exhausted and the seek doesn't throw and error (at least on Windows). This PR fixes this by populating an iterator as we read the file the first time. Same low-memory behavior, slightly slower.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7711fe1e9045`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.