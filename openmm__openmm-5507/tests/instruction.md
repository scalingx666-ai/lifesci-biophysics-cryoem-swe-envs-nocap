# Issue

**Title:** Pack arguments to avoid limits of Metal

## Description

Metal only allows a kernel to have a maximum of 31 arguments.  To work around that, we can pack all the arguments into a block of memory and pass them as a single argument.

This is adapted from an implementation by @RedesignedRobot.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d3e32b4daea1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.