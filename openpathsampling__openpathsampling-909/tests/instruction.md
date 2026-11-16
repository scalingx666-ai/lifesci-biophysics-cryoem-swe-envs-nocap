# Issue

**Title:** Gromacs engine/example updates

## Description

Main improvements:

* Major rewrite of the Gromacs example notebooks to provide a lot more explanation, and to use newer, more convenient approaches within OPS
* The grompp and mdrun command strings are now class variables, making them more easily accessible from outside. The commands to create the fully formatted strings have not changed (that's the API).
* The default values for filenames (before file number is set) are more informative, and include the directories where things end up.
* Gromacs tests are better at cleaning up after themselves
* Added logging conf file to facilitate engine debugging

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3e64d751de98`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.