# Issue

**Title:** Keep modern gromacs files

## Description

Files under src/gromacs/applied_forces/colvars are now de facto maintained in the gromacs repo. Updating them from the Colvars repo tends to break the interface.

As far as I can see, the directory could be scrapped from the repo entirely, and the update script could deal with the library only.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ae72b7b3732a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.