# Issue

**Title:** Add bonds to amber/* and charmm/CYSP

## Description

The impropers were disconnected because of missing C-+N bonds in amber, and a -C-N bond in charmm/CYSP.

This fixes the erroneous info messages. Does someone who actually uses gmx know whether this will result in duplicate bonds?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c8e8b5ec019d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.