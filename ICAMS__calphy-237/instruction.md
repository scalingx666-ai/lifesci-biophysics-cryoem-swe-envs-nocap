# Issue

**Title:** Extend external LammpsLibrary interface

## Description

After some additional testing I noticed that the default setting of closing the LAMMPS library after a given simulation task is confusing when using an external LammpsLibrary interface. So in this pull request I propose to just clear the state of the library rather than closing the library interface completely. Still this comes at the cost that the user has to close the library at the end.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91eb7afea751`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.