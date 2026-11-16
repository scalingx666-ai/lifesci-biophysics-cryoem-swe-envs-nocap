# Issue

**Title:** fix the polar virial

## Description

This is a long-known issue: the trace of the polar virial tensor
in epolar1c/d routines is correct, but the individual components
of the virial tensor do not match the numerical finite difference.
It is now fixed by adding the missing self-torque contribution.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7db4f34d0158`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.