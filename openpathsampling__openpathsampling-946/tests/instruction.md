# Issue

**Title:** fix for filename counter of gromacs engine

## Description

. As proposed there I slightly modified the test to reset the counter via its own `reset()` method instead of via the engines `set_filenames()` method. 
Engine works and test pass with and without the counter reset in `set_filenames()`, so it is now a matter of preference if we want to have that reset in `set_filenames()`. I left it in for now, was not sure what your preference is here.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ee973341b73`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.